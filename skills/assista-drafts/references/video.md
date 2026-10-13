# Making a short video for Assista

You describe a video as a **timeline**: one of four hand-built templates, a
variant, a format, a length and the copy that fills it. Assista makes the
video, checks it and puts it in the writer's approvals. You never write
Remotion, HTML or a URL, and you never publish.

## Order of work

1. **`get_brand_kit`**: the palette slots, the approved asset ids and a
   `video` block with the templates, formats, limits and format lines. If it
   answers `brand: null`, stop and pass its message on.
2. Pick a template and fill the copy. Start from the example below.
3. **`submit_video`** with `{ caption, channels?, timeline }`: one video per
   call. Channels: `linkedin` (default), `x`, `instagram`, `facebook`,
   `tiktok`, `youtube`. Threads takes no video.

## Templates

| Template | Uses | Notes |
|---|---|---|
| `announcement` | headline, detail, cta, photo | the news, then the detail, then the end card |
| `quote` | headline (the quote), attribution, cta | set in beats of up to twelve words |
| `stat-reveal` | stat (required, ≤12 chars), headline, detail, cta | the number counts up and stays |
| `carousel` | headline (title), 2–8 slides, cta | each slide `{ headline ≤90, detail ≤140 or null }` |

`variant` is `stacked` (everything on the ground colour) or `split` (an accent
field or an approved photo holds the hero at the top). `format` is `9x16`,
`4x5` or `1x1`. `fps` is always 30; `durationMs` is 4,000 to 30,000.

## Rules the checks enforce

- **Every field is present.** A field the template does not use is `null`.
- **Copy limits:** headline 140, detail 160, cta 32, attribution 60, stat 12.
- **Colours are palette slots** (`bg`, `fg`, `accent`, `neutral`) in
  `colors: { ground, ink, accent }`; `ink` is the text colour and must differ
  from `ground`, and the two must contrast.
- **Images are approved asset ids** from `get_brand_kit`, or `null`.
- **Long enough to read.** Each screen holds at most twelve words and stays up
  for about three words a second. A video too short for its words is refused;
  lengthen it or cut words.

A timeline that fails a check is refused with the issues and nothing is
saved. Fix and resend; no need to ask the user.

## Example

```json
{
  "version": 1,
  "template": "stat-reveal",
  "variant": "stacked",
  "format": "9x16",
  "fps": 30,
  "durationMs": 13000,
  "colors": { "ground": "bg", "ink": "fg", "accent": "accent" },
  "copy": {
    "headline": "of our customers came from a friend's recommendation.",
    "detail": "No ads. Just people telling people.",
    "cta": "Tell a friend",
    "attribution": null,
    "stat": "42%"
  },
  "assets": { "logo": null, "photo": null },
  "slides": null,
  "audio": "none"
}
```

## After submitting

`submit_video` answers `ok`, the two verdicts (`readable`, `on_brand`), any
warnings, and a link. Tell the user: "Your video is being made and is waiting
for your approval: <url>". It is not posted until they approve it in Assista.
