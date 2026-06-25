tasks from `masters/`:
# current
```dataview
TASK
WHERE !completed AND regexmatch("^masters/*", file.folder) and !frozen
GROUP BY file.link
```
# frozen
```dataview
TASK
WHERE !completed AND regexmatch("^masters/*", file.folder) and frozen
GROUP BY file.link
```