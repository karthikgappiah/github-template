# Agent instructions

## Commits, issues, and pull requests

- **Format:** Use the [Conventional Commits Specification](https://www.conventionalcommits.org/en/v1.0.0/) for all commit messages, issue titles, and pull request titles.
- **Length:** Write multi-line commit messages by default.
- **Scope:** Keep all commits, issues, and pull requests atomic. Do not put unrelated changes together.
- **Trailers:** Claude must not add commit trailers.

## Agent skills

| Topic         | Summary                                       | Reference                      |
| ------------- | --------------------------------------------- | ------------------------------ |
| Issue tracker | This repo uses GitHub Issues and the `gh` CLI. | `docs/agents/issue-tracker.md` |
| Labels        | This repo uses the default labels.            | `docs/agents/triage-labels.md` |
| Domain docs   | This repo has one context.                    | `docs/agents/domain.md`        |
