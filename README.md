# SQL Notes

My study notes from the [AprendeSQL](https://www.aprendesql.dev/) course by midudev, plus a few related database notes. Published as a static site with [Quartz](https://quartz.jzhao.xyz/).

Live site: https://cobos-bioinfo.github.io/sql-notes/

## How it works

The notes live in `content/` as plain Markdown (exported from my Obsidian vault). Every push to `main` triggers the GitHub Actions workflow in `.github/workflows/deploy.yml`, which builds the site and publishes it to GitHub Pages.

## Adding or updating notes

1. Edit or drop Markdown files into `content/`.
2. Commit and push to `main`.
3. The site rebuilds and redeploys automatically.

## Building locally

```bash
npm install
npx quartz build --serve
```

Then open http://localhost:8080.
