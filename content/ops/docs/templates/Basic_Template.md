<%*
const folder = "ops/docs/logs/daily";
const name   = `Daily Log ${tp.date.now("YYYY-MM-DDTHH_mm")}.md`;
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
title: "<% tp.date.now('YYYY-MM-DDTHH_mm') %> Daily Log"
created: "<% tp.date.now('YYYY-MM-DDTHH:mm') %>"
tags:
  - log/daily
---

## Summary
- Date: <% tp.date.now('YYYY-MM-DD') %>
- Day of week: <% tp.date.now('dddd') %>
- Vault: <% tp.file.folder(true) %>
- File: <% tp.file.title %>

## Notes
Start writing here.

## Next steps
- [ ] Task 1
- [ ] Task 2