I am trying to get correct hook working for rsync
needs to be post-merge on main branch

also switch to main branch, would like better warning - os message, not toast
but other hooks create message in toast 

### Reset local main to match GitHub
Follow these from the repo root.
#### Standard reset
```
# abort any in-progress merge (safe to run)
git merge --abort 2>/dev/null || true
# switch to main
git switch main
# fetch the latest remote state
git fetch origin
# make local main identical to origin/main
git reset --hard origin/main
# remove any untracked files and folders (optional but common)
git clean -fd
```
Expected:
```
git status
# On branch main
# Your branch is up to date with 'origin/main'.
# nothing to commit, working tree clean
```
#### One-liner
```
git switch main && git fetch origin && git reset --hard origin/main && git clean -fd
```
#### Optional: keep a backup before wiping
```
git branch rescue-main-`date +%Y%m%d-%H%M%S`
```
Then run the standard reset.
