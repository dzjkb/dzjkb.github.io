    tasks from `masters/`:
```dataview
TASK
WHERE !completed AND regexmatch("^masters/*", file.folder)
GROUP BY file.link
```
