<!-- Copilot/agent instructions for working in this repo -->
# Copilot instructions — google.aip.dev
<!-- Copilot/agent instructions for working in this repo -->
# Copilot instructions — google.aip.dev

This repository is a static site generator for API Improvement Proposals (AIPs). The source is Markdown plus small YAML metadata; an external generator produces the `out/` site used for GitHub Pages.

- **Big picture:**
  - Content lives in `aip/` (AIPs by area) and `pages/` (site pages, templates). See [aip/general/0001.md](aip/general/0001.md) for an example AIP.
  - Site configuration and small site metadata live in `config/` (`header.yaml`, `hero.yaml`, `urls.yaml`).
  - A generator CLI (`aip-site-generator`) transforms source into the static `out/` directory used for publishing.

- **Build & local dev:**
  - Install dependencies: `pip install -r requirements.txt` (the generator is pinned here).
  - Generate site locally: `aip-site-gen . out` (CI uses the same command).
  - Preview: `./serve.sh out` or point any static server to `out/`.
  - Docker: a `Dockerfile` is present for containerized builds.
  - IMPORTANT: do not commit generated content in `out/`.

- **CI & deploy:**
  - CI runs `pip install -r requirements.txt` then `aip-site-gen . out` (see `.github/workflows/test.yaml`).
  - Publish workflow builds `out/` and deploys with `jamesives/github-pages-deploy-action@v4` (see `.github/workflows/publish.yaml`).

- **Environment & secrets:**
  - Local form tests or previewing forms may require `HCAPTCHA_SECRET` — see `.env.example` and CI secrets configuration.

- **Repo conventions & patterns to follow:**
  - Add or edit an AIP: create or edit `aip/<area>/<NNNN>.md` and update `aip/<area>/scope.yaml` for ordering/categories.
  - Page templates: `pages/*.md.j2` use Jinja templating. Many pages have paired YAML metadata files (example: `pages/news/2020-03.md` + `pages/news/2020-03.yaml`). See [pages/general/adopting.md](pages/general/adopting.md).
  - Small edits: change the Markdown/YAML source, run `aip-site-gen . out`, then `./serve.sh out` to verify.
  - Structural changes (template or config) must be validated by regenerating the site and visually inspecting `out/`.

- **Integration points & external dependencies:**
  - `aip-site-generator` (installed via `requirements.txt`) is the single point of templating logic — search for its docs if you need to modify generation behavior.
  - GitHub Pages deployment expects the `out/` folder produced by generation; CI invokes the same generator command.

- **Where to look first when navigating the codebase:**
  - Content: `aip/` and `pages/`
  - Site config: `config/header.yaml`, `config/hero.yaml`, `config/urls.yaml`
  - CI: `.github/workflows/` (test and publish workflows)
  - Local preview helper: `serve.sh`

- **Quick actionable examples:**
  - Add AIP: create `aip/myarea/8000.md` → update `aip/myarea/scope.yaml` →
    ```
    pip install -r requirements.txt
    aip-site-gen . out
    ./serve.sh out
    ```
  - Edit news item: modify `pages/news/2020-03.md` and `pages/news/2020-03.yaml` → regenerate and preview.

- **Code ownership & reviewers:**
  - Check [`.github/CODEOWNERS`](.github/CODEOWNERS) to find likely reviewers for area-specific changes.

If anything here is unclear or you want more specifics (for example, generator invocation options, where templates are defined, or the typical AIP frontmatter fields), tell me which area to expand and I will iterate.
