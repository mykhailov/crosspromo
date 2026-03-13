# AGENTS.md

Instructions for future work in this repository.

## Repo Purpose

This repo stores cross-promo JPEG art that games load directly from GitHub.
Treat filenames and paths as potentially live production URLs.

## Image Rules

- Preserve existing dimensions for the asset slot unless the user explicitly asks to change them.
- Do not rename image files casually. A rename can break live asset URLs.
- Prefer JPEG for the existing asset set unless the user asks for another format.
- Keep icons around `256x256`, portraits around `500x750` or `533x800`, and landscapes around `800x533` unless a specific folder already uses something else intentionally.
- Do not re-export with heavier quality settings unless the user asks for higher quality.

## Required Optimization Workflow

When image files are added or updated:

1. Check the changed files with `git status --short` and `git diff --stat`.
2. Verify dimensions with `sips -g pixelWidth -g pixelHeight`.
3. Run lossless optimization with `jpegtran -copy none -optimize`.
4. Ensure image files do not have executable bits set.
5. Keep `.DS_Store` and local workspace files out of commits.

## Preferred Commands

Check dimensions:

```bash
sips -g pixelWidth -g pixelHeight path/to/file.jpg
```

Lossless optimize in place:

```bash
tmp="file.jpg.tmp.$$"
jpegtran -copy none -optimize -outfile "$tmp" "file.jpg" && mv "$tmp" "file.jpg"
```

Clear executable bits from images:

```bash
find . -type f \( -iname '*.jpg' -o -iname '*.jpeg' \) -perm -111 -exec chmod 644 {} +
```

## Commit and Push

- If only image payload changed, prefer a narrow commit limited to the touched asset files.
- HTTPS push may fail in this environment during `git-receive-pack`.
- Prefer pushing with SSH:

```bash
git push git@github.com:mykhailov/crosspromo.git main
```
