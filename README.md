# InspectFlow

A single-page quality inspection app: templates, a report dashboard, and a
step-by-step inspection wizard (header → phases → result & sign-off), with
Pass/Fail/N/A checkpoints, required comments on any Fail or N/A, and a
downloadable/emailable report in the PSI template layout.

## Files

- `index.html` — the entire app (HTML + CSS + JS, no build step, no dependencies).

## Deploying on GitHub

1. Create a repo and add `index.html` (and this `README.md`) to it.
2. **GitHub Pages**: repo Settings → Pages → deploy from the branch/root. Your
   app will be live at `https://<user>.github.io/<repo>/`.
3. Nothing else to install — it's a static file.

## Important: capability differences outside Claude

This file was built to run as a **published Claude Artifact**, which grants it
extra platform capabilities at runtime (`window.claude.use(...)`). Those calls
are wrapped in `try/catch`, so the app **degrades gracefully** rather than
breaking — but the following only work inside claude.ai, not on GitHub Pages
or any other host:

| Feature | On claude.ai | On GitHub Pages |
|---|---|---|
| Templates & reports storage | Shared database, synced live across everyone who opens the artifact | Falls back to `localStorage` — saved only in that one browser, on that one device |
| Settings → distribution list, edit permission by role | Enforced by the artifact's share settings (owner/editor vs. viewer) | Anyone can edit it locally; nothing is actually shared |
| "Synced · shared database" badge | Shows real sync state | Will show "Local only" |

If you want real shared storage on your own GitHub-hosted version, you'll need
to add your own backend (e.g. Firebase, Supabase, or a small API) and swap out
the `initStore()` / `saveTemplate()` / `saveReport()` functions near the top
of the `<script>` block for calls to it — the rest of the app (UI, wizard
logic, validation, report generation) doesn't depend on where the data lives.

## Email / download

"Send to inspector" and "Email distribution list" download the report as a
`.doc` file (HTML formatted to match the PSI template) and then open a
pre-addressed `mailto:` draft — the browser can't attach the file
automatically, so the file has to be attached by hand before sending. This
works the same on GitHub Pages as it does on claude.ai.
