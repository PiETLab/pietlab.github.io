### Repository architecture and workflow for PiETLab website
#### Overview
The PiETLab web publishing workflow involves **two coordinated Git repositories**:

|**Repository**|**Visibility**|**Role**|**Location**|
|---|---|---|---|
|pietlab-site-private|Private|Source and content development (Obsidian vault, Markdown, assets)|GitHub: https://github.com/PiETLab/pietlab-site-private|
|pietlab.github.io|Public|Quartz-generated static site for public web hosting|GitHub: https://github.com/PiETLab/pietlab.github.io|
All content originates in the **private repo**, is developed and reviewed there, and then built/published to the **public Quartz repo** for deployment to https://pietlab.github.io/.
### Local setup
On the maintainer's local machine:

| **Directory**                           | **Description**                                        |
| --------------------------------------- | ------------------------------------------------------ |
| ~/Documents/Vaults/pietlab-site-private | Local clone of the private development repository      |
| ~/Documents/Vaults/pietlab.github.io    | Local clone of the public Quartz publishing repository |
A local build or sync process copies (or exports) approved Markdown content and assets from pietlab-site-private into the working tree of pietlab.github.io, where Quartz generates the final static site.
### Branch model in pietlab-site-private
pietlab-site-private uses a **protected main branch** and **feature branches** for contributors.

| **Branch**         | **Purpose**                                     | **Access**                   | **Typical Actions**                                              |
| ------------------ | ----------------------------------------------- | ---------------------------- | ---------------------------------------------------------------- |
| main               | Canonical content; merges only via reviewed PRs | Protected (no direct pushes) | Receives merged pull requests after review                       |
| dev-melanie        | Maintainer’s working branch                     | Write                        | Personal development branch; all changes merged into main via PR |
| dev-jessica, dev-* | Contributor branches                            | Write                        | Contributor develops and opens PR to main                        |
This model enforces a consistent workflow and review process while still allowing local experimentation and drafts.
### Branch protection settings
Configured under **Settings → Branches → Branch protection rules** for main:
- Require pull request before merging
- Require linear history
- Restrict direct pushes (only maintainers may bypass if explicitly configured)
- Optionally require status checks (e.g., build, lint, or formatting checks)
### Pull request flow
1. Contributor creates or updates a feature branch (e.g., jessica).
2. Pushes changes to GitHub.
3. Opens a pull request targeting main.
4. Maintainer (Melanie) reviews and merges the PR.
5. GitHub Actions or a manual script builds/publishes the merged content.
### Pull request flow
1. **Contributor creates or updates a feature branch** (for example, jessica).
2. **Contributor adds or edits content** —  by using the **GitHub web interface** (for example, editing Markdown files or uploading new ones directly in the branch) (for advanced contributors, could also do this by committing from a local clone).
3. **Contributor opens a pull request** targeting the main branch.
4. **Maintainer (Melanie)** reviews the pull request and, once approved, merges it into main.
5. **GitHub Actions** or a **manual build script** then regenerates the Quartz site and publishes the updated content.
### Publication workflow
1. **Develop**
    - All drafting, editing, and review occur in pietlab-site-private.
    - Dataview or Obsidian-only content remains internal; public-ready Markdown is finalized for export.
2. **Merge to main**
    - Approved content merges into main.
3. **Build and export**
    - Local or automated build step runs Quartz within the pietlab.github.io working directory.
    - Static files are regenerated and committed.
4. **Deploy**
    - Pushing to main in pietlab.github.io triggers GitHub Pages deployment.
    - The website updates at https://pietlab.github.io.
### Maintainer roles

| **Role**                                                                | **Responsibility**                                                      |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| Maintainer (Melanie)                                                    | Oversees repo structure, branch protection, PR review, and final merges |
| Contributor (e.g., Jessica)                                             | Works within assigned branch, submits PRs for review                    |
| Automation (implemented by Melanie, runs locally on Melanie's computer) | Runs content sync and Quartz build scripts between repos                |
### Future extensions
- Add pre-publish hooks or GitHub Actions to automate Quartz builds.
- Introduce structured YAML front matter validation in pietlab-site-private.
- Implement automated sync scripts to copy only approved content from pietlab-site-private → pietlab.github.io.

