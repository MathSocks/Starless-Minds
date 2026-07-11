---
dg-publish: true
---
```dataview
TABLE SessionNum, AssociatedPlayer as "Associated Player", Recipient
FROM "2. Mechanics/Memories List"
WHERE file.name != "Memories List"
SORT Order asc
```


```dataviewjs
const folder = "2. Mechanics/Memories";
const players = ["Divario", "Faolan", "Folssa", "Ghesh", "Sean", "Bud"];

// Make this an array:
const pages = dv.pages(`"${folder}"`).values;

function countField(field, player) {
    if (!field) return 0;
    if (Array.isArray(field)) return field.filter(p => p === player).length;
    return field === player ? 1 : 0;
}

let associatedRow = ["Associated Player"];
let recipientRow = ["Recipient"];

for (let player of players) {
    let assocCount = pages.reduce((sum, p) => sum + countField(p.AssociatedPlayer, player), 0);
    let recipCount = pages.reduce((sum, p) => sum + countField(p.Recipient, player), 0);

    associatedRow.push(assocCount);
    recipientRow.push(recipCount);
}

dv.table(["", ...players], [
    associatedRow,
    recipientRow
]);
```
