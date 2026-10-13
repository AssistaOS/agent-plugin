# Composing graphics for Assista

You describe a graphic as a **scene**: a flat list of shapes, text and images
on a 1080 × 1080 canvas. Assista draws it, checks it and puts it in the
writer's approvals. You never write SVG, HTML or a URL, and you never publish.

## Order of work

1. **`get_brand_kit`**: the brand name, typography, the four palette slots,
   the six templates, the layer format and the approved asset ids. If it
   answers `brand: null`, stop and pass its message on: the writer has to set
   up their brand in Assista first.
2. Pick a template per graphic and compose the scene from the format lines.
   Start from an example below and move things, rather than from nothing.
3. **`submit_graphics`** with up to six graphics, each
   `{ caption, copy, scene, channels? }`.

## The template catalogue

`templateId` is one of the six `get_brand_kit` lists, with a one-line note on
each: `corner-circle`, `side-band`, `photo-top`, `headline-center`,
`quote-card`, `stat-hero`. The id says which layout you started from; the
layers decide what is drawn. Use `photo-top` only when there is an approved
photo to put in it.

## Rules the checks enforce

- **Palette slots, never hex.** Every `fill` and `stroke` is `bg`, `fg`,
  `accent` or `neutral`. The canvas is painted `bg` first; later layers draw
  on top of earlier ones.
- **At most 24 layers** per scene. A carousel is `{ "slides": [ … ] }` with 2
  to 10 scenes.
- **Every field is required.** Use `null` where the format allows it (a rect
  with no stroke has `"stroke": null`), `rotation: 0` and `opacity: 1` when
  you mean nothing special.
- **Text comes from copy fields** where it can: `"copy": "headline"` (or
  `detail`, `cta`, `brand`) with `"text": null`. Each graphic's `copy` object
  carries `headline`, `detail` and `cta`; `brand` is always the brand name.
  For a carousel, pass `copy` as an array, one object per slide. Use a literal
  `"text"` (up to 120 characters, with `"copy": null`) only for something that
  is not copy, such as the big number in `stat-hero`.
- **Text fits and breathes.** Keep text at least 40px from every edge. A line
  holds about `w / (0.6 × size)` characters and takes `1.2 × size` pixels, so
  size the box and `maxLines` for the words you put in it.
- **Contrast.** Text on `bg` in `fg` (or `bg` on `accent`, as `stat-hero`
  does) reads; text in a slot close to the one under it fails.
- **Images are approved asset ids** from `get_brand_kit`'s `assets`, never a
  URL. An id that is not there fails the on-brand verdict.
- **Name the brand** in every scene: a text layer with `"copy": "brand"`.

## What comes back

Per graphic: `ok`, `post_id`, `url`, two verdicts (`readable`, `on_brand`) and
`issues`. A scene that does not parse, or a caption that breaks one of the
writer's rules, comes back `ok: false` with the issues and nothing is saved
for it: fix it and send that one again. A graphic that is saved with a verdict
of `false` is in the writer's approvals anyway; say which issue to fix.

End with the sentence the tool gives you, in this pattern:

> N graphics are ready for your approval: <url>

## Example: `corner-circle`

```json
{"version":1,"templateId":"corner-circle","canvas":1080,"layers":[
  {"kind":"ellipse","x":760,"y":40,"w":280,"h":280,"rotation":0,"opacity":1,"fill":"accent","stroke":null,"strokeWidth":0},
  {"kind":"text","x":80,"y":80,"w":600,"h":40,"rotation":0,"opacity":1,"copy":"brand","text":null,"fill":"fg","font":"sans","weight":700,"size":28,"align":"left","maxLines":1},
  {"kind":"text","x":80,"y":360,"w":920,"h":330,"rotation":0,"opacity":1,"copy":"headline","text":null,"fill":"fg","font":"sans","weight":700,"size":64,"align":"left","maxLines":4},
  {"kind":"text","x":80,"y":740,"w":920,"h":130,"rotation":0,"opacity":1,"copy":"detail","text":null,"fill":"fg","font":"sans","weight":400,"size":30,"align":"left","maxLines":3},
  {"kind":"text","x":80,"y":920,"w":600,"h":60,"rotation":0,"opacity":1,"copy":"cta","text":null,"fill":"fg","font":"sans","weight":700,"size":32,"align":"left","maxLines":1}
]}
```

## Example: `stat-hero`

```json
{"version":1,"templateId":"stat-hero","canvas":1080,"layers":[
  {"kind":"rect","x":0,"y":0,"w":1080,"h":540,"rotation":0,"opacity":1,"fill":"accent","stroke":null,"strokeWidth":0,"radius":0},
  {"kind":"text","x":80,"y":140,"w":920,"h":260,"rotation":0,"opacity":1,"copy":null,"text":"3×","fill":"bg","font":"sans","weight":700,"size":200,"align":"left","maxLines":1},
  {"kind":"text","x":80,"y":600,"w":920,"h":200,"rotation":0,"opacity":1,"copy":"headline","text":null,"fill":"fg","font":"sans","weight":700,"size":56,"align":"left","maxLines":2},
  {"kind":"text","x":80,"y":820,"w":920,"h":110,"rotation":0,"opacity":1,"copy":"detail","text":null,"fill":"fg","font":"sans","weight":400,"size":28,"align":"left","maxLines":3},
  {"kind":"text","x":80,"y":960,"w":600,"h":40,"rotation":0,"opacity":1,"copy":"brand","text":null,"fill":"fg","font":"sans","weight":700,"size":28,"align":"left","maxLines":1}
]}
```
