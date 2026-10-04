# TOFIX

Findings from a code scan on 2026-10-04.

## Medium

- `README.md:5` - says the `compare` binary renders through pdf-writer, but `src/main.rs` has no pdf-writer path (only krilla, svg2pdf, printpdf, rsvg-convert, marp, chrome); either add a pdf-writer bake or drop it from the README.
- `Cargo.toml:15` - `pdf-writer = "0.14"` is declared but never used anywhere in `src/main.rs`; remove the dependency (and refresh Cargo.lock) or add the missing bake.
- `RECOMMENDATION.md:53` - the verdict and the benchmark row at `RECOMMENDATION.md:116` describe printpdf 0.7 / 0.9, but the harness now builds printpdf 0.12 (`Cargo.toml:16`, `src/main.rs:34`); re-run `./run.py` and update the printpdf verdict and numbers so the document matches what the code measures.
- `viewer.py:54` - `background: #111cc` is an invalid 5-digit hex colour, so the nav bar background is dropped by the browser; use `#111c` or `#111111cc`.

## Low

- `run.py:9` - `import os` is unused; remove it.
- `src/main.rs:174` - printpdf page size is hardcoded to 1280x720 pt regardless of the input SVG (and parse `warnings` at `src/main.rs:170` are discarded), so any other sample is laid out wrongly; take the size from the parsed SVG like the krilla path does.
- `RECOMMENDATION.md:65` - cites `svg/courses/networking/networking-basics/01_tcp_ip/the_tcp_ip_protocol_stack.svg`, a path from another repo; point it at `samples/the_tcp_ip_protocol_stack.svg` which is what `run.py:17` actually uses.
- `Cargo.toml:1` - the repo has no `rsconstruct.toml` and no `.github/workflows/build.yml`, so nothing builds or lints it in CI unlike the rest of the fleet; add the standard build config.
