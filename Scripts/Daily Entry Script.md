<%*
const DAILY_FOLDER = "LabNote/Daily Entries";
const EXP_FOLDER = "LabNote/Experiments";
const ONGOING_MARKER = "#OnGoingExperiments";

const todayName = window.moment().format("YYYY-MM-DD");
const dailyPath = `${DAILY_FOLDER}/${todayName}`;

// If today's daily already exists: open it and stop.
// (Since we are already inside a newly created note, we also close that note.)
const existing = app.vault.getAbstractFileByPath(dailyPath);
if (existing) {
  const leaf = app.workspace.getLeaf(true);
  await leaf.openFile(existing);

  const fe = app.workspace.getLeavesOfType("file-explorer")?.[0];
  if (fe?.view?.revealInFolder) fe.view.revealInFolder(existing);

  // close current scratch note (the one created by this command)
  app.workspace.getMostRecentLeaf()?.detach();
  tR = "";
  return;
}

// IMPORTANT: rename/move FIRST (avoid races with "saving")
await tp.file.rename(todayName);
await tp.file.move(dailyPath);

// Collect ongoing experiments
const expFiles = app.vault.getMarkdownFiles().filter(f => f.path.startsWith(EXP_FOLDER));
let items = [];

for (const f of expFiles) {
  const content = await app.vault.cachedRead(f);
  if (!content.includes(ONGOING_MARKER)) continue;

  const cache = app.metadataCache.getFileCache(f);
  const fm = cache?.frontmatter ?? {};

  const basename = f.basename;
  const fallbackCode = basename.split(/\s+/)[0];
  const code = (fm.Code ?? fm.code ?? fallbackCode).toString().trim();

  items.push({ code, basename });
}

items.sort((a, b) => a.code.localeCompare(b.code));

const blocks = items.map(({ code, basename }) =>
`### [[${basename}]]
${code}:: 
`).join("\n");

const now = window.moment().format("YYYY-MM-DD HH:mm");

// Let Templater write the note content (NO app.vault.modify)
tR =
`---
modification date: ${now}
tags:
  - DailyEntries
---

\`\`\`dataview
TABLE WITHOUT ID
  file.name AS "OnGoingProject", Name AS "Name", Project AS "Project"
FROM #OnGoingExperiments
WHERE file.name != "Daily Entry Script"
  AND file.name != "Experiment template"
SORT file.name ASC
\`\`\`

${blocks}

![[Scheduler#TODO]]

## Daily Log
- 
`;
%>