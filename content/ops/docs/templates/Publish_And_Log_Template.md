<%*
const folder = "ops/docs/logs/publish";
const name   = `Publish Log ${tp.date.now("YYYY-MM-DD_HH_mm")}.md`;
const target = `${folder}/${name}`;

try {
  // Ensure destination folder exists
  if (!app.vault.getAbstractFileByPath(folder)) {
    await app.vault.createFolder(folder);
  }

  // Only move/rename if needed
  if (tp.file.path !== target) {
    await tp.file.move(target);
  }

  // Optional: leave a breadcrumb in the note body for debugging
  //tR = `<!-- moved-to: ${target} -->\n`;
} catch (e) {
  // Surface any failure visibly in the note
  tR = `<!-- move-failed: ${String(e)} (from: ${tp.file.path} to: ${target}) -->\n`;
}
-%>
---
title: "Publish Log <% tp.date.now('YYYY-MM-DD_HH_mm') %>"
created: "<% tp.date.now('YYYY-MM-DD_HH:mm') %>"
tags:
  - publish/log
  - website
source_repo: pietlab-site-private
public_repo: pietlab.github.io
---

## Summary
- Date: <% tp.date.now('YYYY-MM-DD') %>
- Time: <% tp.date.now('HH:mm') %>
- Mode: manual local publish
- Source: `pietlab-site-private@main` → `pietlab.github.io@main`

## Command
`<%* 
// Adjust the script path and args as needed.
const script = "/Users/mb/Documents/Vaults/pietlab-site-private/ops/bin/publish_to_public.sh";
// Render the command we are about to run
const cmd = `${script}`;
tR += cmd; 
%>

## Output

## Notes
- git user.name: <%*
const { execSync } = require("child_process");
const gitUser = execSync("git config user.name").toString().trim();
tR += gitUser;
%>
- Observations: 
- Follow-ups: