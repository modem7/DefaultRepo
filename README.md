# DefaultRepo

Blueprint/template for new repositories. Primarily targeting Linux-based projects.

---

## Things to update in a new repo

| File | What to change |
|---|---|
| `.wakatime-project` | Set the project name |
| `.github/settings.yml` | Update repo name, description, homepage, topics |
| `renovate.json` | Usually nothing — extends the shared `modem7/renovate-config` preset. Override `assignees`/`labels`/`timezone` or add optional extras here if this repo needs something different |
| `LICENSE` | Set the copyright year to the current year (format: `Copyright (c) <year> modem7`) |
| `README.md` | Replace with actual documentation |

---

## Files included

| File | Purpose |
|---|---|
| `.gitattributes` | Short line-ending rules: LF default, CRLF for Windows scripts, binary list |
| `.gitignore` | Common ignore patterns (OS, editors, secrets, build artifacts, language-specific) |
| `.editorconfig` | Consistent indentation and encoding across editors (LF, UTF-8) |
| `.github/settings.yml` | Repo config-as-code via [GitHub Settings App](https://github.com/apps/settings), inheriting the shared label set and defaults from [`modem7/.github`](https://github.com/modem7/.github) via `_extends: .github`. The Sponsor button (`FUNDING.yml`) is inherited from the same repo, so it is not copied here |
| `.github/workflows/autoassign.yml` | Auto-assigns new issues to maintainer |
| `.github/PULL_REQUEST_TEMPLATE.md` | PR description template |
| `.github/ISSUE_TEMPLATE/` | Bug report and feature request forms |
| `renovate.json` | Automated dependency updates via [Renovate](https://docs.renovatebot.com/), extending the shared [`modem7/renovate-config`](https://github.com/modem7/renovate-config) preset |
| `.github/CODEOWNERS` | Auto-requests review from @modem7 on all PRs |
| `CONTRIBUTING.md` | Contribution guidelines |
| `LICENSE` | MIT License |
| `.wakatime-project` | WakaTime project identifier |

---

## Renovate config

`renovate.json` extends the shared [`modem7/renovate-config`](https://github.com/modem7/renovate-config) preset — `config:recommended` plus the standard assignee/label/timezone settings and automerge for low-risk CI plumbing updates. Add these alongside the default extend when this repo needs them (see that repo's README for details):

- `github>modem7/renovate-config:docker` — for repos with a Dockerfile using s6-overlay and `ARG ALPINE_VERSION` / `ARG PYTHON_VERSION` pins
- `github>modem7/renovate-config:automergePatch` — automerge patch updates, not just digests
- `github>modem7/renovate-config:automergeDigest` — automerge digest updates outside office hours (already included via `automergePatch`)

Repo-specific rules (e.g. version pins, path exclusions) belong in this file, extending the shared config rather than duplicating it.

---

## Git line endings

### List files that rely only on the `* text=auto` fallback

```bash
git ls-files | git check-attr text --stdin | grep ': auto$'
```

Anything listed has no per-type rule. Git still detects text or binary on its own, so add a rule only if it picks wrongly.

### Inspect line endings in the index

Shows index (`i/`), worktree (`w/`), and `.gitattributes` (`attr/`) line ending state per file — useful for diagnosing mismatches before renormalizing:

```bash
git ls-files --eol
```

### Find files with CRLF already committed

```bash
git ls-files --eol | grep "i/crlf"
```

### Renormalize existing files

```bash
git add . -u
git commit -m "Saving files before refreshing line endings"
git add --renormalize .
git status
git commit -m "Normalize all the line endings"
```

---

## References

- [GitHub: Configuring git to handle line endings](https://docs.github.com/en/get-started/getting-started-with-git/configuring-git-to-handle-line-endings#per-repository-settings)
- [gitattributes templates](https://github.com/alexkaratarakis/gitattributes)
- [GitHub Settings App](https://github.com/repository-settings/app)
- [Renovate docs](https://docs.renovatebot.com/)
- [modem7/renovate-config](https://github.com/modem7/renovate-config) — shared Renovate presets extended by this repo
