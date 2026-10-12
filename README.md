# Acorn Creative theme updates

Public update channel for the **Acorn Creative parent theme**. This repo holds
build artifacts only — no source. The theme's source lives in the private
`Acorn-Creative/acorn-creative-theme` repo.

Client sites poll the metadata file below through WordPress's native update flow
(via [plugin-update-checker](https://github.com/YahnisElsts/plugin-update-checker),
vendored in the theme):

```
https://acorn-creative.github.io/theme-updates/acorn-creative-theme.json
```

It points at the newest `acorn-creative-theme-X.Y.Z.zip` in this directory.

## Contents

| File | What it is |
| --- | --- |
| `acorn-creative-theme.json` | Update metadata — version, download URL, details URL. Rewritten each release. |
| `acorn-creative-theme-X.Y.Z.zip` | A release build. **Old versions are kept** so an earlier one stays installable for a rollback. |
| `.nojekyll` | Stops GitHub Pages' Jekyll step from filtering any release file. |

## Publishing

Don't commit here by hand. Releases are cut from the source repo:

```bash
./release.sh 1.1.0
```

That bumps the version, builds, tags, packages a clean zip with `git archive`,
then copies the zip + JSON into a local clone of this repo, commits
`Release 1.1.0`, and pushes. It reads that clone's path from `ACORN_UPDATES_REPO`
in the source repo's gitignored `.release.env`.

Two things to expect afterwards: Pages takes about a minute to rebuild, and each
client site caches its update check for 12 hours — Dashboard → Updates →
**Check again** forces it.

## Why this is public

WordPress downloads the zip without credentials, so the channel has to be
publicly reachable no matter where it is hosted. Keeping the artifacts here and
the source private is the same exposure as any self-hosted channel, with no
server to maintain. Do not make the source repo public, and do not embed an
access token in shipped theme code to work around this.
