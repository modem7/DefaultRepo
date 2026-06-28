# DefaultRepo

Blueprint/template for new repositories. Primarily targeting Linux-based projects.

---

## Things to update in a new repo

| File | What to change |
|---|---|
| `.wakatime-project` | Set the project name |
| `.github/settings.yml` | Update repo name, description, homepage, topics |
| `renovate.json` | Adjust timezone if needed |
| `README.md` | Replace with actual documentation |

---

## Files included

| File | Purpose |
|---|---|
| `.gitattributes` | Universal line-ending rules for 200+ file types (LF default) |
| `.gitignore` | Common ignore patterns (OS, editors, secrets, build artifacts, language-specific) |
| `.editorconfig` | Consistent indentation and encoding across editors (LF, UTF-8) |
| `.github/settings.yml` | Repo config-as-code via [GitHub Settings App](https://github.com/apps/settings) |
| `.github/workflows/autoassign.yml` | Auto-assigns new issues to maintainer |
| `.github/workflows/labelsync.yml` | Syncs labels from config file on push or manual trigger |
| `.github/config/labels.yml` | Label definitions (single source of truth) |
| `.github/PULL_REQUEST_TEMPLATE.md` | PR description template |
| `.github/ISSUE_TEMPLATE/` | Bug report and feature request forms |
| `renovate.json` | Automated dependency updates via [Renovate](https://docs.renovatebot.com/) |
| `.github/CODEOWNERS` | Auto-requests review from @modem7 on all PRs |
| `.github/FUNDING.yml` | Adds "Sponsor" button linking to Buy Me a Coffee |
| `CONTRIBUTING.md` | Contribution guidelines |
| `LICENSE` | MIT License |
| `.wakatime-project` | WakaTime project identifier |

---

## Git line endings

### Check for missing .gitattributes rules

```bash
missing_attributes=$(git ls-files | git check-attr -a --stdin | grep "text eol=lf")
if [[ "$missing_attributes" ]]; then
  echo ".gitattributes rule missing for the following files:"
  echo "$missing_attributes"
else
  echo "All files have a corresponding rule in .gitattributes"
fi
```

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
