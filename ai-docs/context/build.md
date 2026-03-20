# Build & Run

## Local Development

No build process. Open `index.html` directly in a browser, or serve locally:

```bash
# Python
python -m http.server 8000

# Node
npx serve .
```

Then visit `http://localhost:8000`.

## Deployment

Push to `main` branch. GitHub Pages auto-deploys to `red-phoenix.team`.

No CI/CD pipeline, no build step, no environment variables required.

## Linting / Formatting

No linter or formatter configured. The project is a single HTML file.
