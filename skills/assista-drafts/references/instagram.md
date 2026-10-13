# Instagram

## Hard limits

- **2,200 characters** in the caption.
- **An image is required.** Instagram does not take text-only posts: one image
  or a carousel of 2 to 10. `submit_drafts` refuses a text-only Instagram
  draft (`needs-image`). Send it as a graphic instead: `submit_graphics` with
  `channels: ["instagram"]` (see `graphics.md`).
- Links in captions are not clickable, and Assista's checks refuse a URL in
  the body. Say "link in bio" if the link matters.

## Writing for it

- The caption sits under the image. Write it to go with a picture: say what
  the picture cannot.
- Only the first line or two show before "more", roughly the first 125
  characters. Put the hook there.
- Short paragraphs with blank lines between them. Line breaks render as
  written.
- 50 to 150 words is a good default. Long captions work when they tell a story.

## Norms

- Hashtags: Assista allows at most three, in the final paragraph. Pick
  specific ones over broad ones (`#saasfounders`, not `#business`).
- Emoji are normal here, in moderation. Never as bullet points: that is
  flagged.
- A closing line that asks for a specific reply or a save works better than
  "Thoughts?".
