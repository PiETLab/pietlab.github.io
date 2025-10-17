<%*
const folder = "ops/docs/logs/publish";
const name   = `Daily Log ${tp.date.now("YYYY-MM-DD_HH_mm")}.md`;
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
title: "Publish Log {{date:YYYY-MM-DD}}"
created: "{{date:YYYY-MM-DDTHH:mm}}"
tags:
  - publish/log
  - website
source_repo: pietlab-site-private
public_repo: pietlab.github.io
---

## Summary
- Date: "<% tp.date.now('YYYY-MM-DD_HH:mm') %>"

- Date: {{date:YYYY-MM-DD}}
- Mode: manual local publish
- Source: `pietlab-site-private@main` → `pietlab.github.io@main`

## Command
`<%* 
// Adjust the script path and args as needed.
const script = "/Users/mb/Documents/Vaults/pietlab-site-private/ops/bin/publish_to_public.sh";
// Render the command we are about to run
const cmd = `${script}`;
tR += cmd; 
%>`

## Output
<%*
/*
Templater’s tp.system runs a shell command and returns stdout+stderr as a string.
Make sure Templater → User system commands is enabled.
*/
let output = "";
try {
  output = await tp.system(cmd);
} catch (e) {
  output = `ERROR: ${e}`;
}
tR += "\n```console\n" + output + "\n```\n";
%>

## Notes
- Operator: "<% tp.user.name() %>"

- Operator: `{{user_name}}`
- Observations: 
- Follow-ups: