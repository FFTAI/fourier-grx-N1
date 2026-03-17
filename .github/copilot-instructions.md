# Copilot Instructions

## Build, test, and lint commands

This repository does not define checked-in build, test, or lint commands. There is no `Gemfile`, `package.json`, `Makefile`, or test suite in the repo.

Treat changes here as documentation-site changes:

- Validate Jekyll front matter fields instead of inventing new tooling.
- Keep internal links and asset paths consistent with the existing site-wide absolute path style such as `/fourier-grx-N1/docs/...` and `/fourier-grx-N1/assets/...`.
- There is no repo-local single-test command because no test framework is checked in.

## High-level architecture

This repository is a GitHub Pages / Jekyll documentation site for the Fourier GRX N1 SDK, not the SDK implementation itself.

- `_config.yaml` configures a `just-the-docs` remote theme and site behavior such as search, sticky navigation, heading anchors, and the GitHub auxiliary link.
- `index.md` is the landing page and explains the three SDK interface layers that drive the rest of the documentation:
  - **User API**: higher-level application development over Zenoh.
  - **Developer API**: lower-level Python library access for direct robot control.
  - **PubSub API**: Zenoh-based publish/subscribe interface modeled after the Developer API.
- Top-level pages in `docs/` are section index pages: `quickstart.md`, `examples.md`, `reference.md`, `tasks.md`, `training.md`, `user_case.md`, `usage.md`, `faq.md`, `update.md`, `changelog.md`.
- Subdirectories under `docs/` hold the child pages for those sections:
  - `docs/quickstart/` for onboarding by robot type.
  - `docs/examples/` for User / Developer / PubSub example usage.
  - `docs/reference/` for interface contracts, run modes, config files, command-line tooling, and system behavior.
  - `docs/tasks/` for task and module reference pages keyed by task IDs (TID) and module IDs (MID).
  - `docs/training/` and `docs/user_case/` for specialized workflows and scenarios.
- The runnable examples referenced by this site are mostly in the external `Wiki-GRx-Deploy` repository on branch `FourierN1`; this repo mainly documents how to use them.
- Many reference pages document runtime files that live on the robot host, especially under `~/fourier-grx/`, so avoid treating those paths as repository files.

## Key conventions

- Keep new pages in the existing Jekyll front matter style:
  - `layout: default` on every page.
  - top-level section pages use integer `nav_order`.
  - child pages use decimal `nav_order` and a `parent` matching the Chinese section title exactly.
- Table-of-contents usage is inconsistent by design and should match nearby pages:
  - some pages use `toc: true` with `toc_min_header` / `toc_max_header`.
  - others use `has_toc: true` plus the literal body block `* TOC` and `{:toc}`.
  - when editing an existing page, preserve its current TOC pattern rather than normalizing it.
- Internal links use site-root absolute paths beginning with `/fourier-grx-N1/...` rather than relative links.
- Image references also use absolute `/fourier-grx-N1/assets/images/...` paths. Some Mermaid-heavy pages include a fallback static image link in the prose; keep that pattern when updating diagram pages.
- Content language is primarily Simplified Chinese. New content should follow the surrounding page language unless there is a strong reason not to.
- The documentation is version- and mode-sensitive:
  - run modes (`debug`, `sdk`, `server`, etc.) are central to how User / Developer / PubSub pages fit together.
  - task IDs and module IDs can change by robot model or `fourier-grx` version, so prefer wording like “use the values documented on the current task page” instead of hard-coding assumptions beyond the referenced page.
  - Developer and PubSub docs explicitly split behavior for `4.0.0` and above versus older versions.
- Preserve the important units distinction in API docs:
  - newer Developer / PubSub docs use radians-based units for joint and IMU motion data.
  - older pre-`4.0.0` sections document degree-based units.
- The repo’s “architecture” is documentation taxonomy, not source modules. When adding or editing docs, keep the cross-links aligned across:
  - run modes in `docs/reference/run_type.md`
  - config/runtime behavior in `docs/reference/config_file.md` and `docs/reference/system_running_logic.md`
  - task semantics in `docs/tasks*.md`
  - example execution steps in `docs/examples*.md`
- `README.md` is intentionally minimal, and contributor guidance is in `docs/contributing.md`; do not expect a separate development handbook elsewhere in the repo.
