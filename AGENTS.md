# Repository guidance

- `skills/graduation-defense/` is the installable skill and single source of truth. Keep its relative references portable.
- `examples/` and tests use explicitly fictional material; do not commit actual school templates, thesis drafts, student information or private project data.
- The helper exports basic drafts. Do not claim exact school-template preservation, rendered visual quality, or factual validation from file generation alone.
- Run `python -X utf8 -m unittest discover -s tests -v` and the example `audit` command after helper changes. Output artifacts belong in ignored `build/` or a user-specified work directory.
