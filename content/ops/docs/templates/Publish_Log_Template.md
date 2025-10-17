---
title: "Publish Log {{date:YYYY-MM-DD}}"
created: "{{date:YYYY-MM-DDTHH:mm}}"
tags:
- publish/log
- website
source_repo: pietlab-site-private
public_repo: pietlab.github.io
---

<%*
const logFolder = "ops/docs/logs/publish";
const fileName = `Publish Log ${tp.date.now("YYYY-MM-DD")}`;
const fullPath = `${logFolder}/${fileName}.md`;

if (!tp.file.exists(fullPath)) {
  await tp.file.create_new(tp.file.find_tfile(tp.config.template_file), fullPath);
  return;
}
%>

## Summary
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
- Operator: `{{user_name}}`
- Observations: 
- Follow-ups: