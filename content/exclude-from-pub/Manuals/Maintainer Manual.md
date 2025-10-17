# Maintainer Manual
## 1. Create branches for content contributors
- Create branches for content contributors - convention is `dev-<name of contributor>`
Use a personal branch. Do not commit directly to main.
```
cd ~/Documents/Vaults/pietlab-site-private
# start from the latest main
git checkout main
git pull origin main
# create a personal branch (first time only)
git checkout -b dev-melanie   # replace with your own branch name if you are not Melanie
git push -u origin dev-melanie
```
## 2. Give contributors permission
- Give contributors permission.
  In GitHub:
	- Go to the repo → **Settings → Collaborators and teams**.
	- Add contributor (search by username or email).
	- Give at least **Write** access.
## 3. Protect main branch
#### purpose
The `main` branch in `pietlab-site-private` is the canonical record of all reviewed and approved content.  
Branch protection prevents direct pushes or accidental overwrites and ensures that all changes reach `main` through pull requests (PRs).
#### preferred configuration: Classic Branch Protection Rule
The PiETLab organization currently uses GitHub’s **Free plan**, where the newer “Rulesets” feature is available for configuration but **not enforceable on private repositories** without a paid **Team** or **Enterprise** plan.  
Therefore, the **classic branch protection rule** is the correct and fully supported choice.
##### steps
##### 1. Navigate to your repository:  
   `https://github.com/PiETLab/pietlab-site-private`
##### 2. Set up Branch protection rule
Go to **Settings → Branches**.
Under **Branch protection rules**, click  
   **Add classic branch protection rule**.
In **Branch name pattern**, enter:
```
main
```
##### 3. Configure the protection options
When you create the rule for main, configure the following fields and checkboxes exactly as listed below.
###### 1. Protect matching branches

|**Setting**|**Action**|**Notes**|
|---|---|---|
|**Require a pull request before merging**|✅ Enable|All updates must flow through a pull request.|
|**Require approvals**|✅ Enable|Set _Required number of approvals before merging_ to **1** (you may self-approve).|
|**Dismiss stale pull request approvals when new commits are pushed**|☐ Leave unchecked|Not required unless you want approvals invalidated after every commit.|
|**Require review from Code Owners**|☐ Leave unchecked|Optional; only use if you maintain a CODEOWNERS file.|
|**Restrict who can dismiss pull request reviews**|☐ Leave unchecked|Only needed for large multi-reviewer teams.|
|**Allow specified actors to bypass required pull requests**|☐ Leave unchecked|Keep consistency by enforcing the same rules for all users.|
|**Require approval of the most recent reviewable push**|☐ Leave unchecked|Optional safeguard, not required for small teams.|
###### 2. Additional merge requirements

|**Setting**|**Action**|**Notes**|
|---|---|---|
|**Require status checks to pass before merging**|☐ Leave unchecked|Enable later if you add CI or lint checks.|
|**Require conversation resolution before merging**|☐ Optional|Good practice if multiple reviewers.|
|**Require signed commits**|☐ Leave unchecked|Optional; only needed for strict signature workflows.|
|**Require linear history**|✅ Enable|Prevents merge commits; enforces a clean, linear history.|
|**Require deployments to succeed before merging**|☐ Leave unchecked|Only for advanced CI/CD setups.|
|**Lock branch**|☐ Leave unchecked|Use only when freezing all work on main.|
|**Do not allow bypassing the above settings**|✅ Enable|Ensures even admins follow the rule set.|
|**Restrict who can push to matching branches**|✅ Enable|Add your username (@mbaljko) so only you can push directly if absolutely necessary.|
###### 3. Rules applied to everyone including administrators

|**Setting**|**Action**|**Notes**|
|---|---|---|
|**Allow force pushes**|☐ Leave unchecked|Prevents rewriting main history.|
|**Allow deletions**|☐ Leave unchecked|Prevents accidental branch deletion.|
After all options are set, click **Create** (or **Save changes**).

This configuration enforces a consistent review workflow while keeping main fully protected against accidental edits, force pushes, or deletions.

After this rule is created, any attempt to push directly to `main` will fail:
```
remote: error: GH006: Protected branch update failed for refs/heads/main.
````
All changes must now flow through PRs.
### Alternative (not currently in use): Branch Rulesets
GitHub introduced **Rulesets** as the successor to classic branch protection.  
Rulesets offer finer-grained control and can apply to multiple branches, tags, or repositories across an organization.  
However, on the **Free plan**, rulesets for **private repositories** cannot be enforced.  
Attempting to create one displays:
> “Your rulesets won't be enforced on this private repository until you upgrade this organization account to GitHub Team.”
#### To use Rulesets in the future
- Upgrade the PiETLab organization to **GitHub Team**.  
- Then use **Settings → Rules → Add branch ruleset**,  
  specifying:
  - Target branch pattern: `main`  
  - Require pull request before merging  
  - Restrict direct pushes  
  - Prevent deletions and force pushes  
Rulesets will then replace classic protections for all repositories under the organization.
### verification
To confirm that protection is active:
```bash
git checkout main
echo "test" >> tmp.txt
git add tmp.txt
git commit -m "test commit"
git push origin main
````
Expected result:
```
remote: error: GH006: Protected branch update failed for refs/heads/main.
```
Protection is now enforced, and all contributions must use the approved pull-request workflow.
## Working as a Maintainer - Reviewing PRs 

go to GitHub website, do it there



## Syncing Content: Upstream main branch has been revised and now the website should update

the deployment logic lives in .git/hooks/post-merge

this means:

- Whenever you run git pull on main, and that pull updates your branch (either by fast-forward or by true merge), the post-merge hook fires.
