# Crosspromo Art

This repository stores cross-promo art that is loaded by games directly from GitHub.

## Structure

Each game has its own folder with a small set of JPEG assets:

- `icon*.jpg` or `icon*.jpeg`
- `portrait*.jpg`
- `landscape*.jpg` or `ads_landscape*.jpg`
- `bootup*.jpg` for boot splash promo variants when needed

## Recommended Sizes

Keep the existing project conventions unless a game requires something else:

- icon: `256x256`
- portrait: `500x750` or `533x800`
- landscape: `800x533`

## Before Commit

When updating images:

1. Export as JPEG in the same dimensions used by the existing asset slot.
2. Do not keep PSD/Photoshop metadata or EXIF if it is not needed.
3. Keep filenames stable if games load them directly from GitHub URLs.
4. Avoid accidental extra variants unless they are really needed.
5. Make sure image files are not executable.
6. Run lossless optimization before commit.

## Lossless JPEG Optimization

Lossless optimization makes JPEG files smaller without changing the visible image.
It removes unnecessary metadata and rewrites JPEG tables more compactly.

For a single file:

```bash
jpegtran -copy none -optimize -outfile output.jpg input.jpg
```

For in-place optimization inside this repo:

```bash
find . -type f \( -iname '*.jpg' -o -iname '*.jpeg' \) -print0 | \
while IFS= read -r -d '' f; do
  tmp="$f.tmp.$$"
  jpegtran -copy none -optimize -outfile "$tmp" "$f" && mv "$tmp" "$f"
done
```

## Repo Hygiene

- `.DS_Store` and editor workspace files should not be committed.
- Keep naming consistent where possible.
- If a new asset becomes the canonical one, remove stale `_old` files when safe.
