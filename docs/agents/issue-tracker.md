# Issue tracker: GitHub

This repo keeps issues and specs in GitHub Issues. Use the `gh` CLI for all operations.

`gh` gets the repo from `git remote -v`. Run `gh` in a clone of this repo.

## Issue operations

| Operation       | Command                                                     |
| --------------- | ----------------------------------------------------------- |
| Create an issue | `gh issue create --title "..." --body "..."`                |
| Read an issue   | `gh issue view <number> --comments`                         |
| List issues     | See [List issues](#list-issues).                            |
| Add a comment   | `gh issue comment <number> --body "..."`                    |
| Add a label     | `gh issue edit <number> --add-label "..."`                  |
| Remove a label  | `gh issue edit <number> --remove-label "..."`               |
| Close an issue  | `gh issue close <number> --comment "..."`                   |

Use a heredoc for a body that has more than one line.

When you read an issue, use `jq` to filter the comments. Also get the labels.

### List issues

Add `--label` and `--state` filters as necessary.

```bash
gh issue list --state open --json number,title,body,labels,comments --jq '[.[] | {number, title, body, labels: [.labels[].name], comments: [.comments[].body]}]'
```

## Skill terms

| Skill instruction                | Action                              |
| -------------------------------- | ----------------------------------- |
| "Publish to the issue tracker"   | Create a GitHub issue.              |
| "Fetch the relevant ticket"      | Run `gh issue view <number> --comments`. |

## Pull requests as a triage surface

**PRs as a request surface: no.**

Set this flag to `yes` if this repo uses external PRs as feature requests. `/triage` reads this flag.

If the flag is `yes`, PRs use the same labels and states as issues. Use these commands:

| Operation                 | Command                                                |
| ------------------------- | ------------------------------------------------------ |
| Read a PR                 | `gh pr view <number> --comments`                       |
| Read a PR diff            | `gh pr diff <number>`                                  |
| Add a comment             | `gh pr comment`                                        |
| Add or remove a label     | `gh pr edit --add-label` or `gh pr edit --remove-label` |
| Close a PR                | `gh pr close`                                          |

To list external PRs for triage:

1. Run this command:

   ```bash
   gh pr list --state open --json number,title,body,labels,author,authorAssociation,comments
   ```

2. Keep only the PRs that have one of these `authorAssociation` values: `CONTRIBUTOR`, `FIRST_TIME_CONTRIBUTOR`, `NONE`.

Issues and PRs use the same number sequence. Thus, `#42` can be an issue or a PR. To find the type:

1. Run `gh pr view 42`.
2. If this command fails, run `gh issue view 42`.

## Wayfinding operations

`/wayfinder` uses these operations. The **map** is one issue. Each **child** issue is one ticket.

| Item         | Instruction                                                                                     |
| ------------ | ----------------------------------------------------------------------------------------------- |
| Map          | Run `gh issue create --label wayfinder:map`. The body has Notes, Decisions-so-far, and Fog.     |
| Child ticket | Link the issue to the map as a GitHub sub-issue. Use `gh api` on the sub-issues endpoint.       |
| Child labels | Use `wayfinder:research`, `wayfinder:prototype`, `wayfinder:grilling`, or `wayfinder:task`.     |
| Assignee     | Assign a claimed ticket to the developer who drives the session.                                |

If sub-issues are not available:

1. Add the child to a task list in the map body.
2. Put `Part of #<map>` at the top of the child body.

### Blocking

Use GitHub native issue dependencies. To add a dependency:

1. Get the database ID of the blocker. Do not use the `#number` or the `node_id`.

   ```bash
   gh api repos/<owner>/<repo>/issues/<n> --jq .id
   ```

2. Add the dependency to the child:

   ```bash
   gh api --method POST repos/<owner>/<repo>/issues/<child>/dependencies/blocked_by -F issue_id=<blocker-db-id>
   ```

`issue_dependencies_summary.blocked_by` shows only the open blockers.

If dependencies are not available, put a `Blocked by: #<n>, #<n>` line at the top of the child body.

A ticket is unblocked when all its blockers are closed.

### Find the next ticket

1. List the open children of the map. Use `gh issue list --state open`, and limit the list to the sub-issues or the task list of the map.
2. Remove each ticket that has an open blocker. An open blocker is `issue_dependencies_summary.blocked_by > 0`, or an open issue in the `Blocked by` line.
3. Remove each ticket that has an assignee.
4. Select the first remaining ticket in map order.

### Claim a ticket

Run `gh issue edit <n> --add-assignee @me`. Do this as the first write of the session.

### Resolve a ticket

1. Run `gh issue comment <n> --body "<answer>"`.
2. Run `gh issue close <n>`.
3. Add a context pointer (gist and link) to Decisions-so-far in the map.
