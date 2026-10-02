# Dockerfile Linter

Paste a Dockerfile and get a list of best-practice warnings, each with a severity and a suggested fix. It is a static heuristic linter that reads your file line by line in the browser. No server, no tracking, no external dependencies.

**Live demo:** https://0xelitesystem.github.io/dockerfile-linter/

## Features

- Line-by-line parsing that honors backslash line continuations
- Severity levels (error, warning, info) with a per-run summary count
- A concrete suggested fix for every finding
- Checks include:
  - Base image not pinned to a specific tag or digest (using `latest`)
  - Running as root (no `USER` instruction)
  - `ADD` used where `COPY` would suffice
  - `apt-get install` without cleanup or `--no-install-recommends`
  - Missing `.dockerignore` hint
  - Many separate `RUN` layers that could be combined
  - Missing `HEALTHCHECK`
  - Secrets or keys hardcoded in `ENV` or `ARG`
  - `COPY . .` before installing dependencies (busts the layer cache)
  - No `WORKDIR`
  - Missing `EXPOSE`
  - Use of `sudo`
- Dark-mode toggle, keyboard usable (Ctrl or Cmd + Enter lints)

## How it works

The tool splits your Dockerfile into logical instructions (joining lines that end with a backslash), then pattern-matches each instruction against a set of well-known Dockerfile smells. Whole-file checks (like a missing `USER` or `HEALTHCHECK`) run after the per-line pass. Everything is plain string and regex work in JavaScript. It does not build, run, or resolve your image, so it cannot validate registry digests or the contents of your actual `.dockerignore`. Treat it as a fast first pass, not a guarantee.

## Use

1. Paste a Dockerfile into the Dockerfile box, or click Load sample to see one with problems.
2. Click Lint, or press Ctrl or Cmd + Enter.
3. Read each finding with its severity and suggested fix, plus the summary count by severity.
4. Fix the file and lint again. Clear empties the box.

## Why this exists

Most Dockerfile mistakes (unpinned base images, running as root, cache-busting COPY order) are cheap to catch before a build. This catches the common ones in a single HTML file with no install, no tracking and no upload, under the MIT license.

## Privacy

Everything runs in your browser. The Dockerfile you paste never leaves your machine. You can confirm this by viewing the page source or watching the network tab in DevTools, no requests are made. The tool works offline with no external dependencies.

If you use the theme toggle, your light or dark choice is saved in your browser's localStorage under the key `theme`. Nothing you paste or type is stored.

## Run locally

```
git clone https://github.com/0xelitesystem/dockerfile-linter
cd dockerfile-linter
```

Open `index.html` in a browser. Or serve the folder with `python -m http.server` and visit http://localhost:8000.

## Build

No build step. The whole tool is one `index.html` file with inline CSS and JavaScript, and no dependencies.

## More

Part of a catalog of single-file browser tools and plain-language references, all MIT licensed and dependency-free: [0xelitesystem.github.io](https://0xelitesystem.github.io/). Built by [elitesystem.ai](https://elitesystem.ai).

## License

MIT. Copyright 0xelitesystem 2026.
