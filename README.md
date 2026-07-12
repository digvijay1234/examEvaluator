# Answer Sheet Evaluator

A fully client-side OMR-style exam evaluator. No backend, no server, no database —
everything (PDF answer-key parsing, scoring) runs in the visitor's browser.

## Why no backend?

GitHub Pages only serves static files, so there's no server to run backend code on
anyway. That's not a limitation here — this tool doesn't need one. PDF parsing is
done with `pdf.js` running entirely client-side, and results never leave the
browser tab.

The `vendor/pdfjs/` folder contains a **self-hosted copy** of the pdf.js library,
instead of loading it from a CDN at runtime. This matters more than it might seem:
during testing, an external CDN script getting blocked (by a mobile network,
ad-blocker, or in-app browser) silently broke the entire page. Serving it from
your own repo removes that failure mode completely — once GitHub Pages serves the
file, everything the page needs ships with it.

## Deploying

1. Push this folder's contents to a GitHub repo (keep `index.html` and `vendor/`
   at the same level).
2. In the repo, go to **Settings → Pages**.
3. Under "Build and deployment", set **Source** to "Deploy from a branch", pick
   your branch (e.g. `main`) and folder (`/root` if this is the repo root, or
   `/docs` if you put it in a `docs/` folder).
4. Save. GitHub gives you a URL like `https://<username>.github.io/<repo>/`
   within a minute or two.

No build step, no `npm install`, no server config — it's plain HTML/CSS/JS.

## File structure

```
index.html              the entire app (UI + logic)
vendor/pdfjs/
  pdf.min.js             pdf.js library (self-hosted, not loaded from a CDN)
  pdf.worker.min.js       pdf.js background worker
```

## Updating pdf.js later

If you ever want a newer pdf.js version, download the two files above from a
release of https://github.com/mozilla/pdf.js and drop them into `vendor/pdfjs/`,
replacing the existing ones. No code changes needed as long as the filenames stay
the same.
