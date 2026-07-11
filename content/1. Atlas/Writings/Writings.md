---
Status: Incomplete
dg-publish: true
---

### Academic Writings
```dataview
TABLE Author
FROM "1. Atlas/Writings" and #Writing
WHERE Type = "Academic"
SORT Author asc, file.name desc
```

### Creative/Personal Writings
```dataview
TABLE Author
FROM "1. Atlas/Writings" and #Writing
WHERE Type = "Correspondence"
SORT Author asc, file.name desc
```