# Content Contributor Manual (Obsidian-based workflow)

This adapts the workflow to the Obsidian Git plugin. 

Repository: pietlab-site-private. 

Your personal branch: dev-melanie (contributors substitute their branch name, for example, jessica).
## Prerequisites
- Obsidian installed with the **Git** community plugin enabled.
- The vault folder is a Git repo linked to origin (git@github.com:PiETLab/pietlab-site-private.git).
- You can authenticate to GitHub via SSH.
- The default branch is main (policy-protected, even if not technically enforced on Free plan).
### plugin basics you will use
Use the Command Palette (Cmd/Ctrl+P) and run:
- Git: Checkout → switch branches
- Git: Pull → fetch and merge from the remote tracking branch
- Git: Add & commit all changes → commit everything with a message
- Git: Push → push current branch
- Git: Show status → quick status view
Obsidian cannot open pull requests; use GitHub Web (or gh CLI) for PRs.
## Pick your working branch in Obsidian
1. Open the command palette (Cmd/Ctrl + P) and run
    **Git: Switch branch**.
    This opens a list of all branches (for example, main, dev-melanie, jessica).
2. Select your personal branch (for example, dev-melanie).
    The branch name now appears in the Obsidian status bar (bottom right).
3. To pull the latest changes from GitHub, run
    **Cmd/Ctrl + P → Git: Pull**.
Contributors substitute their own branch names (for example, dev-jessica) in the same steps.

## Verify you are on the correct branch
The Obsidian Git plugin shows your current branch and sync state directly in the **status bar** (bottom right corner of the Obsidian window).
- The **branch name** (for example, dev-melanie) appears in the status bar.
- Small icons beside the branch name indicate sync state:
    - ↑1 — your branch is **ahead** of the remote (you have 1 local commit to push).
    - ↓1 — your branch is **behind** the remote (you need to pull).
    - ↑1↓2 — you have commits both to push and to pull.
    - ✔ — your branch is up to date.
You can hover the mouse over this indicator to confirm details.

### make and commit changes safely
1. Edit Markdown and assets in Obsidian.
2. Review changes: Git: Show status.
3. Commit:
    - Git: Add & commit all changes
    - Enter a clear message, for example: Add: workshop notes on XYZ
4. Push: Git: Push.
Tips
- Commit focused, logical units.
- Ignore volatile Obsidian files. Ensure .gitignore contains:
```
.obsidian/workspace.json
.obsidian/workspace-mobile.json
.obsidian/appearance.json
```
- If any of these were previously tracked, untrack once from the command line:
```
git rm --cached .obsidian/workspace.json .obsidian/workspace-mobile.json .obsidian/appearance.json
git commit -m "Stop tracking Obsidian UI-state files"
git push
```
### keep your branch current with main
When main changes, bring those updates into your personal branch:
Merge option (simple)
1. Git: Checkout → dev-melanie (if not already on it)
2. In a terminal (merge target must be specified explicitly):
```
git fetch origin
git merge origin/main
git push
```
2. Or, if you prefer to stay in Obsidian, first run Git: Pull on main, then:
    - Git: Checkout → dev-melanie
    - In terminal: git merge origin/main → resolve conflicts if prompted → Git: Push.
Rebase option (linear history)
```
git fetch origin
git rebase origin/main
# resolve conflicts if needed:
git add -A
git rebase --continue
git push --force-with-lease
```
If unsure, use the merge option.



## Create a pull request when ready
1. Ensure your latest changes are pushed from Obsidian (Git: Push on dev-melanie).
2. Open GitHub repo page.
3. Under "Code", Click “Compare & pull request.”
4. Base: main; Compare: dev-melanie.
5. Add a concise title and description; create the PR.
6. Respond to review comments by editing and pushing more commits to dev-melanie; the PR updates automatically.
## After approval and merge
- GitHub merges your PR into main.
- Your publish process runs (GitHub Actions or your manual script).

- Optional local cleanup:
```
git checkout main
git pull origin main
git branch -d dev-melanie
git fetch -p
```
- Then recreate dev-melanie from updated main for the next task:
```
git checkout -b dev-melanie
git push -u origin dev-melanie
```
### working entirely in the GitHub web interface
- Use the branch selector to switch to dev-melanie (or create it).
- Add or edit files; commit directly to that branch.
- Open a pull request to main as above.
### common issues and fixes with the plugin
- .obsidian/workspace.json blocks pull with “would be overwritten by merge”
    - Keep your layout: git stash push --include-untracked -m "obsidian workspace" .obsidian/workspace.json → run Git: Pull → git stash apply.
    - Or discard: delete .obsidian/workspace.json and run Git: Pull; Obsidian will recreate it.
- Need to discard all local edits and match remote exactly (on your current branch):
```
git fetch origin
git reset --hard origin/dev-melanie    # or origin/<your-branch>
git clean -fd
```

- Accidentally committed to main:
```
git checkout -b rescue-main-<date>
git push -u origin rescue-main-<date>
git checkout main
git reset --hard origin/main
```
- Open a PR from `rescue-main-<date>` to main if you need those changes.
### quick checklist before opening a PR
- On your personal branch (dev-melanie for the maintainer).
- Git: Show status reports a clean working tree.
- Your branch includes the latest origin/main (merged or rebased).
- Commit messages are clear and scoped.
- Local preview/build looks correct if applicable.

Let me know if you want this exported as a Markdown file named Contributor_Workflow_Obsidian_Git.md with your vault paths included.
