# Klox CDN

Static assets served to [klox.ai](https://klox.ai/) over jsDelivr.

[Klox](https://klox.ai/) is an AI video creation canvas. An agent drafts the
script, storyboard and shots as a node graph; you open any node to rewrite the
prompt, swap the model or drop in your own footage, and regenerate just that
branch instead of the whole video.

This repository holds the images and video used by the marketing pages, blog
posts and guides on that site. It ships no application code.

## How assets are served

Files are published through jsDelivr's GitHub endpoint, pinned to a release tag:

```
https://cdn.jsdelivr.net/gh/seekton/klox-cdn@v1.0.5/files/pages/blender/usage-1.webp
```

**The tag is load-bearing.** klox.ai references assets at a pinned version, so
committing a new file to `main` does not make it reachable — the pinned URL
keeps serving the tagged tree. Every asset change needs a new tag, and the
consuming code needs its version bumped to match.

jsDelivr caches a tagged URL permanently. Never change the bytes behind a
filename that has already been released; add a new file, or cut a new tag.

## Layout

```
files/
  pages/
    blender/     # assets for /blender and the Blender add-on blog post
    landing/     # assets for the landing page
```

One directory per page, named after the page slug. Both the source image and
the optimized output live side by side (`usage-1.png` next to `usage-1.webp`)
so assets can be regenerated without hunting for the original.

Reference the `.webp`; the `.png` is the source, not the delivery format.

## Scripts

Requires Node and a local `npm install`.

```bash
npm run png          # convert png/jpg/jpeg to webp (sharp)
npm run poster       # extract a poster frame from a video
npm run last-frame   # extract the final frame instead
```

`scripts/png2webp.ts` walks a directory, optionally recursively, and skips
files that are already WebP.

## Releasing

```bash
npm run release      # patch bump, commit, tag, push
```

`release.sh` stages everything, commits, runs `npm version`, and pushes with
`--follow-tags`. Pass a version type and message directly for anything other
than a patch:

```bash
./release.sh minor "add pricing page hero"
```

After pushing the tag, update the pinned version in the consuming code —
`CDN_BASE_URL` in the Klox app — or the new assets will not appear.

## Links

- [Website](https://klox.ai/)
- [Canvas](https://klox.ai/canvas)
- [Blender guide](https://klox.ai/blender)
- [Pricing](https://klox.ai/pricing)
- [Discord](https://discord.gg/36janVUvm4)

Built and operated by [Seekton LLC](https://seekton.com/).
