---
dg-publish: false
dg-home: false
---


### Commands
```meta-bind-button
label: New Session Notes
icon: ":LiFile:"
style: primary
class: ""
cssStyle: ""
tooltip: ""
id: ""
hidden: false
actions:
  - type: templaterCreateNote
    templateFile: System/Templates/Template Pages/Session Template.md
    folderPath: 1. Atlas/Session Notes
    fileName: SESSION -
    openNote: true
    openIfAlreadyExists: false
  - type: command
    command: obsidian-icon-folder:iconize:set-icon-for-file

```

### Guides
- [[Dataview Guide]]
- [[Markdown Guide]]
- [[Templater Sheet]]

### Most Recently Created
```dataview
LIST WHERE file.ctime >= date(today) - dur(1 day)
```

### Most Recently Modified
```dataview
LIST WHERE file.mtime >= date(today) - dur(1 day)
```

### GitHub Repository Token
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