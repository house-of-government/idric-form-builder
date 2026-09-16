# Agent instructions

Before writing or reviewing Idriç in this repository, read:

1. [`STYLE.md`](STYLE.md)
2. the canonical [`isomorphisms/Idric/STYLE.md`](https://github.com/isomorphisms/Idric/blob/Idri%C3%A7/STYLE.md)
3. the canonical [railway](https://github.com/isomorphisms/Idric/tree/Idri%C3%A7/examples/intent/railway) and [HTTP server](https://github.com/isomorphisms/Idric/tree/Idri%C3%A7/examples/intent/http_server) intent examples
4. [`README.md`](README.md) for this repository's form-domain boundary

Do not copy the canonical style guide into this file. `STYLE.md` records local
constraints; the Idriç repository remains the source of truth for language-wide
style.

Work on a branch. Preserve the separation between form semantics and transport,
storage, renderer, or platform adapters. Inspect neighboring work before
inventing a new local pattern, and run the checks relevant to the files changed
before proposing a merge.

Whenever giving the human a script or command block, assume `$PWD` is arbitrary.
Resolve repository and file paths from the script's own location, an explicit
project location, or a discovered repository root, and perform any required
`cd` inside the script. Never require the human to `cd` first or rely on relative
paths against their current working directory.
