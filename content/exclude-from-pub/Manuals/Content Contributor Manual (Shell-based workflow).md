# Content Contributor Manual (Shell-based workflow)

### Working as a content developer
This guide shows how to do day-to-day content work safely using branches and pull requests in the pietlab-site-private repository. 

The maintainer’s personal branch is dev-melanie. Contributors should use their own personal branch (for example, jessica).
### prerequisites
- You have write access to the repository
- Git is installed and your SSH key works with GitHub
- The default branch is main and is protected
### pick  your working branch
Since your branch already exists:
```
git checkout dev-melanie
git pull                       # pulls from origin/dev-melanie because of -u tracking
```
Contributors can substitute their branch name (for example, jessica) in the same commands.
### verify you are on the correct branch
Always check before editing.
```
git status
git branch         # current branch is marked with an asterisk
git branch -vv     # shows tracking (e.g., dev-melanie -> origin/dev-melanie)
```
Expected messages
- on branch dev-melanie (or your personal branch)
- your branch is up to date with origin/dev-melanie
If you see main, switch back to your branch:
```
git checkout dev-melanie
```
### make and commit changes safely
1. edit files in your editor (or Obsidian)
2. review what changed
```
git status
git diff
```
3. stage and commit with a clear message
```
git add path/to/file.md   # or: git add -A
git commit -m "Add: workshop notes on XYZ"
```
4. push to your remote branch
```
git push
```
tips
- commit early and often with focused messages
- avoid committing Obsidian system folders; add entries to .gitignore if needed
### keep your branch current with main
If others have merged to main, bring those changes into your branch regularly.
merge option (simple and safe)
```
git checkout dev-melanie
git fetch origin
git merge origin/main
git push
```
rebase option (linear history)
```
git checkout dev-melanie
git fetch origin
git rebase origin/main
# resolve any conflicts, then:
git push --force-with-lease
```
If you are unsure, prefer the merge option.
### create a pull request when ready
from the command line
```
git push   # ensure latest commits are on origin/dev-melanie
```
then in GitHub
1. open the repository page
2. click compare & pull request (or new pull request)
3. base branch: main
4. compare branch: dev-melanie (or your personal branch)
5. add a concise title and description, then create pull request
add reviewers
- request review from the maintainer (Melanie for most PRs)
- respond to feedback by pushing more commits to the same branch; the PR updates automatically
### after approval and merge
- GitHub merges your PR into main
- the site publishing process runs (GitHub Actions or your build script)
- optionally delete your feature branch on GitHub; locally you can clean up with:
```
git checkout main
git pull origin main
git branch -d dev-melanie                  # deletes local branch (safe if merged)
git fetch -p                                # prunes remote-tracking branches
```
### working via the web interface (no local clone)
- use the branch selector to create or switch to your personal branch (for example, dev-melanie or jessica)
- click add file → create new file or upload files
- commit directly to your branch with a clear message
- open a pull request targeting main as above
### common issues and fixes
- pushed to the wrong branch: create a new branch from that state, then reset the wrong branch
```
git checkout -b dev-melanie-fix
git push -u origin dev-melanie-fix
git checkout main
git reset --hard origin/main
```
-   
- your branch is behind origin/dev-melanie: run git pull
- conflicts during merge or rebase: open conflicted files, edit, then
```
git add .
git commit        # for merges
# or for rebases:
git rebase --continue
git push (or --force-with-lease if you rebased)
```
### quick checklist before opening a PR
- on your personal branch (for Melanie: dev-melanie)
- git status is clean (no unintended changes)
- branch includes latest origin/main (merged or rebased)
- commit messages explain the changes
- build or preview looks correct locally if applicable
