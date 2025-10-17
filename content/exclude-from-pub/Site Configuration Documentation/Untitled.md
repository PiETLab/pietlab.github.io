### **Short answer**

  

Yes—Case A (fast-forward) should apply almost always for you. If you ever touch main locally by mistake (even making commits), you can wipe everything and make your local main exactly match GitHub with a hard reset.

  

### **Panic button: make local** 

### **main**

###  **identical to origin**

  

Run these in your repo root:

```
# If you're mid-merge, abort it first (safe to run even if not merging)
git merge --abort 2>/dev/null || true

# Make sure you're on main
git switch main

# Fetch the latest remote state
git fetch origin

# Hard reset local main to match the remote exactly
git reset --hard origin/main

# Remove any untracked files/dirs created locally
git clean -fd
```

After this:

```
git status
# On branch main
# Your branch is up to date with 'origin/main'.
# nothing to commit, working tree clean
```

### **If you accidentally committed on main and want to keep those edits elsewhere**

  

You can salvage them into a branch before the reset:

```
# from main with your accidental commits
git branch rescue-main-$(date +%Y%m%d-%H%M%S)

# then wipe main as above
git fetch origin
git reset --hard origin/main
git clean -fd
```

Later, if any of that was useful, you can cherry-pick from rescue-main-* onto dev-melanie.

  

### **Make local main “read-only” so it won’t happen again**

  

Add tiny hooks in pietlab-site-private/.git/hooks/ (no extension) and make them executable (chmod +x).

  

#### **pre-commit (block commits on main)**

```
#!/usr/bin/env bash
set -euo pipefail
branch="$(git rev-parse --abbrev-ref HEAD)"
if [[ "$branch" == "main" ]]; then
  echo "Commit blocked: do not commit on main. Switch to your dev branch." >&2
  exit 1
fi
```

#### **pre-push (you already have this, but here’s a hardened version)**

```
#!/usr/bin/env bash
set -euo pipefail
branch="$(git rev-parse --abbrev-ref HEAD)"
if [[ "$branch" == "main" ]]; then
  echo "Push blocked: do not push main. Use a PR from your dev branch." >&2
  exit 1
fi
```

#### **post-checkout (warn when entering main)**

```
#!/usr/bin/env bash
set -euo pipefail
# $3 == 1 means branch checkout (not file checkout)
if [[ "${3:-0}" -eq 1 ]]; then
  branch="$(git rev-parse --abbrev-ref HEAD)"
  if [[ "$branch" == "main" ]]; then
    echo "Warning: you are on main (read-only). Make no edits here."
  fi
fi
```

These hooks also protect you when committing from Obsidian, because the plugin just calls your system git.

  

### **Obsidian-specific tip**

  

Obsidian doesn’t have a “reset hard” command. If you ever need to nuke local main, use the terminal sequence above. Day-to-day, stay on dev-melanie in Obsidian and only switch to main to pull and trigger your post-merge deploy.

  

### **One-liner you can memorize**

```
git fetch origin && git reset --hard origin/main && git clean -fd
```

That’s the “wipe it all away” command for local main.