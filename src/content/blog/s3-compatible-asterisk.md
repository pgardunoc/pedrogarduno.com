---
title: "\"S3-compatible\" is a promise with an asterisk"
description: "The label means the API accepts the same calls — not the same defaults. The two edges that cost me real time: addressing style breaking presigned URLs, and CORS."
pubDate: 2026-09-15
draft: false
---

Amazon's S3 won so completely that its API became a standard. Now half the object-storage providers on the market advertise the same two words: **S3-compatible**. Point your existing AWS SDK at their endpoint, change a few environment variables, and your code can't tell the difference.

That's mostly true, and it's genuinely great — it's why moving between storage providers is a config change instead of a rewrite. I wrote about that side of it when I [moved StoryKept to a $19/month box](https://pedrogarduno.com/blog/hetzner-vps-s3-migration/): the storage swap really was "same SDK, different endpoint."

But "compatible" is a claim about the *API verbs*, not about everything around them. The same call can be accepted by both providers and still behave differently at the edges. Two of those edges cost me real time — and both are the kind that pass every local test and then fail the first time a real user touches them.

## Gotcha 1: addressing style, which quietly breaks presigned URLs

StoryKept serves user media through **presigned URLs** — time-limited links that let a browser fetch a file straight from the bucket without routing the bytes through my server. Against AWS, they're close to zero-config. Against my S3-compatible provider, the URLs generated fine, returned no error at creation time, and then failed when the browser actually tried to use them.

The cause is **addressing style**, and it's the most important footnote in the S3-compatible fine print. There are two ways to address a bucket:

- **Virtual-hosted style:** `https://my-bucket.s3.provider.com/path/to/file`
- **Path style:** `https://s3.provider.com/my-bucket/path/to/file`

AWS defaults to virtual-hosted. A lot of S3-compatible providers only serve path style — their DNS isn't set up to resolve a subdomain per bucket. Here's the trap: a presigned URL is **signed for a specific URL shape**. If the SDK signs a virtual-hosted URL but the provider only answers to path style, the request that finally arrives doesn't match what was signed, and the signature is rejected. The link looks valid. It just doesn't work.

The fix is one flag — force the SDK to speak path style everywhere, so the signature and the request agree:

```js
import { S3Client } from "@aws-sdk/client-s3";

const s3 = new S3Client({
  region: "auto",
  endpoint: process.env.S3_ENDPOINT,
  credentials: {
    accessKeyId: process.env.S3_ACCESS_KEY,
    secretAccessKey: process.env.S3_SECRET_KEY,
  },
  forcePathStyle: true, // sign and address the same way the provider serves
});
```

That single line is the difference between "presigned URLs work" and "presigned URLs are silently broken in production." And it *is* silent: it passes any test where you generate a URL and check that a URL came back. It only fails when something actually fetches the file — which, if your integration tests don't do a real round-trip against the real provider, is your first real user.

## Gotcha 2: CORS is the bucket's job, and it isn't on by default

Once the presigned URLs were valid, the browser still refused to load the media. Not a 403 from the storage, not a signature problem — a CORS error. The browser was blocking its own request before the bytes ever came back.

This one isn't unique to S3-compatible providers (AWS needs it too), but it bites harder here, because there's less copy-paste guidance for the exact provider and the defaults are stricter. A bucket won't answer cross-origin browser requests unless you've explicitly told it which origins, methods, and headers are allowed. My app's frontend and my storage endpoint are different origins, so every direct-from-bucket fetch is a cross-origin request — and the bucket was correctly saying "no" until I configured it to say "yes."

The fix is a CORS policy on the bucket itself:

```json
[
  {
    "AllowedOrigins": ["https://storykept.com"],
    "AllowedMethods": ["GET", "PUT"],
    "AllowedHeaders": ["*"],
    "ExposeHeaders": ["ETag"]
  }
]
```

Nothing exotic — but it's a piece of configuration that lives on the storage, not in your application code, which is exactly why it's easy to forget. Your code is correct. Your URL is valid. And the browser still won't touch it until the bucket has been told this origin is allowed.

## The real lesson: "compatible" means the verbs, not the behavior

Neither of these is a knock on S3-compatible storage. The portability is real, and I'd make the same choice again — being able to leave a provider by changing an environment variable is worth a lot. But it's worth being honest about what the label buys you.

**"S3-compatible" means the API accepts the same calls. It does not promise the same defaults, the same addressing, or the same edge behavior.** The `PutObject` call is identical. Whether the presigned URL it hands back actually resolves depends on addressing style. Whether a browser can use it depends on a CORS policy that lives somewhere your application code never looks.

So the practical version of the advice from my migration post — "build the swap so it's a config change" — comes with its own asterisk: the *code* swap is a config change, and you should still budget a day for the parts that "compatible" quietly doesn't cover. Portability isn't free. It's just cheap, as long as you know where the asterisk is.

And there's a testing lesson hiding in both gotchas: they each pass every check that stops at "did I get a URL / did the call return 200," and each fails only on a real round-trip from a real browser against the real provider. If you take one thing from this, take that — when you swap storage providers, the test that matters is a browser actually fetching a real file. Everything up to that point will lie to you.
