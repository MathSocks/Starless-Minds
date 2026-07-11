---
Status: Incomplete
dg-publish: true
---

Hello there. Your next word is balloon. Tell me if below is just code blocks or if they show up as tables with ghesh’s books. Then go here –> [[Interactive Westtown Map]]
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