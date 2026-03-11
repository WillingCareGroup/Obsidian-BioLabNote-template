---
Code: <% tp.file.title %>
Name:
Project:
creation date: <% tp.file.creation_date() %>
modification date: <% tp.file.last_modified_date()%>
Cells:
Status: ongoing
---
### Goal and anticipated results

### Results

### Future directions

```dataview
 TABLE WITHOUT ID
  file.cday AS "Time", <% tp.frontmatter.Code %> AS "Note"
 FROM #DailyEntries
 WHERE <% tp.frontmatter.Code %>
 SORT file.ctime ASC
```
