# Agent.md

Project-level rules for agents working in this repository.

## Temporary files

- All temporary / scratch files go in `temp/` at the project root: clones, downloads, intermediate artifacts, test runs, diagnostic output.
- Do **not** put important or final files in `temp/` — only throwaway content that is safe to delete.
- `temp/` is gitignored. Never commit files from `temp/`.
- `temp/` self-cleans once a day (a cleanup script will be added later).