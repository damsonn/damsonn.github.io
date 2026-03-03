# Resume

JSON Resume for Damien Dormal, built with [JSON Resume](https://jsonresume.org/) and the [dev-ats](https://www.npmjs.com/package/jsonresume-theme-dev-ats) theme.

## Prerequisites

- [Node.js](https://nodejs.org/)
- [resume-cli](https://github.com/jsonresume/resume-cli) installed globally:
  ```sh
  npm install -g resume-cli
  ```

## Setup

```sh
npm install
```

## Usage

### Generate PDF

```sh
resume export --theme jsonresume-theme-dev-ats resume.pdf
```

### Generate HTML

```sh
resume export --theme jsonresume-theme-dev-ats resume.html
```

### Local preview

```sh
resume serve --theme jsonresume-theme-dev-ats
```

## Structure

- `resume.json` — Resume content (JSON Resume schema v1.0.0)
- `resume.pdf` — Generated PDF output
- `resume.html` — Generated HTML output
