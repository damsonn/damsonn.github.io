# CLAUDE.md

## Project overview

Personal resume built with the JSON Resume standard. Resume content lives in `resume.json` and is rendered to HTML/PDF using `resume-cli` with the `jsonresume-theme-dev-ats` theme.

## Build commands

- **Generate PDF**: `resume export --theme jsonresume-theme-dev-ats resume.pdf`
- **Generate HTML**: `resume export --theme jsonresume-theme-dev-ats resume.html`
- **Local preview**: `resume serve --theme jsonresume-theme-dev-ats`

## Key files

- `resume.json` — All resume content (JSON Resume schema v1.0.0)
- `resume.pdf` / `resume.html` — Generated outputs (regenerate after editing `resume.json`)
- `package.json` — Dependencies (only `jsonresume-theme-dev-ats`)

## Conventions

- After any change to `resume.json`, regenerate both HTML and PDF outputs.
- The theme is `jsonresume-theme-dev-ats` (ATS-optimized, print-friendly, A4 format).
- `resume-cli` is installed globally, not as a project dependency.
