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

Creates a *list* of the specified notes
`dataview
list`

### `From`

Determines **where** to get notes *from*.

#### From #Tag

You can get all notes *from* a specified **tag**:

```dataview
list
from #Party 
```

#### From "Folder"

All notes from a **folder**:

```dataview
list
from "2. Mechanics/Deities"
```

#### From \[\[Links\]\]

And even all notes with links coming *into* a note:

```dataview
list
from [[SESSION 4]]
```

Or going *out of* a note:

```dataview
list
from outgoing([[Command Screen]])
```

- This syntax may change in an upcoming release.

#### Combining Sources

You can use the 3 basic logical operators to create more complex `from` queries:

- `list from #A and #B`
- `list from "University" or "Work"`
- `list from -#Personal`
- `list from [[CSS]] and -#HTML`

#### String Concatenation

In the results of a `list`, you can include metadata fields joined with strings

```dataview
list "File Path: " + file.path + " :)"
from #SN
```

#### Lists of lists

A `list` can also display indented sublists of metadata:

```dataview
list authors
from #SN/Blog
```

### `Task`

`Task` searches for all checkboxes `- [ ] ` in your vault.

It returns a list of all tasks, grouped by their parent note

```dataview
task from #MOC
```

### `Where`

After choosing *which* notes to use, you can narrow down the list further using a `where` block.  
This lets you use the various *comparison operators* on the metadata fields in your notes.

`>`, `>=`, `<`, `<=`, `=`, `!=`

`where {condition}`

#### Examples

- `where file.size > 1000`
- `where file.name != "2021-04-09 Daily Note"`
- `where file.mtime >= date(today) - dur(1 day)`
- `where !complete`

### `Table`

`Table` can show you a *table* of various metadata fields linked to each note.

`Table {field 1}, {field 2}, ...`

#### Examples

```dataview
table intensity
from #Uni/2021/Asg
```

![Dataview table](https://i.imgur.com/OnEoP7J.png)

```dataview
table title, type
from #SN
```

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

```dataview
table authors from #SN
```

versus

```dataview
table authors from #SN
flatten authors
```

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

This is a **nested list** of all the assignments grouped by intensity.  
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

```dataview
table intensity, rows.title
from #Uni/2021/Asg
group by intensity
```

#### Group by tags

```dataview
table rows.file.tags, rows.file.link
from #Fi/Yoga
group by file.tags
```

##### Limitations

It will only consider two notes to be in the same group if they have **exactly the same tags**.

- So even if two notes have `#Note/Author`, if the one has a tag that the other doesn't, they won't be grouped together.