### Implicit Metadata

| Property     | Value                                    | Type     |
| ------------ | ---------------------------------------- | -------- |
| `file.name`  | File **title**                           | `string` |
| `file.path`  | Full file **path**                       | `string` |
| `file.link`  | **Link** to the file                     | `link`   |
| `file.size`  | **Size** (in bytes) of the file          | `number` |
| `file.ctime` | Date that the file was **created**       | `date`   |
| `file.mtime` | Date that the file was last **modified** | `date`   |
| `file.day`   | The **date** contained in the note title | `date`   |
| `file.tags`  | An `array` of all **tags** in the note.  | `array`  |

###

### `List`

Creates a _list_ of the specified notes
`dataview
list`

### `From`

Determines **where** to get notes _from_.

#### From #Tag

You can get all notes _from_ a specified **tag**:

- [[3. Characters/Players/Sean.md|Sean]]
- [[3. Characters/Players/Folssa.md|Folssa]]
- [[3. Characters/Players/Ghesh.md|Ghesh]]
- [[3. Characters/Players/Faolan.md|Faolan]]
- [[3. Characters/Players/Divario.md|Divario]]
- [[3. Characters/Players/Bud.md|Bud]]

#### From "Folder"

All notes from a **folder**:

- [[2. Mechanics/Deities/Godhood Tiers.md|Godhood Tiers]]

#### From \[\[Links]]

And even all notes with links coming _into_ a note:

- [[1. Atlas/Session Notes/SESSION 5.md|SESSION 5]]

Or going _out of_ a note:

- This syntax may change in an upcoming release.

#### Combining Sources

You can use the 3 basic logical operators to create more complex `from` queries:

- `list from #A and #B`
- `list from "University" or "Work"`
- `list from -#Personal`
- `list from [[CSS]] and -#HTML`

#### String Concatenation

In the results of a `list`, you can include metadata fields joined with strings

#### Lists of lists

A `list` can also display indented sublists of metadata:

### `Task`

`Task` searches for all checkboxes `- [ ] ` in your vault.

It returns a list of all tasks, grouped by their parent note

### `Where`

After choosing _which_ notes to use, you can narrow down the list further using a `where` block.\
This lets you use the various _comparison operators_ on the metadata fields in your notes.

`>`, `>=`, `<`, `<=`, !=\`

`where {condition}`

#### Examples

- `where file.size > 1000`
- `where file.name != "2021-04-09 Daily Note"`
- `where file.mtime >= date(today) - dur(1 day)`
- `where !complete`

### `Table`

`Table` can show you a _table_ of various metadata fields linked to each note.

`Table {field 1}, {field 2}, ...`

#### Examples

| File | intensity |
| ---- | --------- |

![Dataview table](https://i.imgur.com/OnEoP7J.png)

| File | title | type |
| ---- | ----- | ---- |

```dataview
table file.tags
from Kw/Yoga
```

### `Sort`

You can use `sort` to define which order to list the results in, and which `field` to sort by:

`sort field asc/desc`

Give multiple fields to decide ties

`sort field1 asc/desc, field2 asc/desc, ...`

### `Flatten`

Use `flatten` to "unroll" lists into their own rows.

| File | authors |
| ---- | ------- |

versus

| File | authors |
| ---- | ------- |

### `Group by`

`Group by` lets do gather together results based on the value of a field.

You may group:

- tasks by `completed`
- games by `rating`
- assignments by `intensity`

First, gather all the assignments:

`from #Uni/2021/Asg`

Then, group by `intensity`:

`group by intensity`

#### `rows` Object

By grouping the notes, we've created a **new object**.

This is a **nested list** of all the assignments grouped by intensity.\
Something like:

```js
[
	[A1, A2, A6], // Green
	[A3, A4], // Yellow
	[A5, A7], // Red
];
```

To access this new list, we use the `rows` object.

- Get the file name of every note in the array: `rows.file.name`
- Get the due date of every note: `rows.dueDate`

| intensity | intensity | rows.title |
| --------- | --------- | ---------- |

#### Group by tags

| file.tags | rows.file.tags | rows.file.link |
| --------- | -------------- | -------------- |

##### Limitations

It will only consider two notes to be in the same group if they have **exactly the same tags**.

- So even if two notes have `#Note/Author`, if the one has a tag that the other doesn't, they won't be grouped together.
