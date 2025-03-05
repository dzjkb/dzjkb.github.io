(that don't refer to my masters thesis)
```dataview
TASK
WHERE !completed AND regexmatch("^masters/*", file.folder) = false
GROUP BY file.link
```