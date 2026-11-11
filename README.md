# Family Tree

A small, personal family tree app: one HTML file, vanilla JS, no build step, no dependencies. Doodle / graph-paper look, pastel and warm.

## Run

Open `family-tree.html` in a browser (or host it anywhere static). Data is saved automatically in the browser's `localStorage` (key `famtree.v1`), so it is per browser and per device. Use **Export all** regularly as a backup.

## Features

- **People:** first / middle / last name, other names, gender (free text with suggestions), born / died (free-text dates, optional times, places), deceased flag, age, significant dates, biography, sources, photo, card colour.
- **Relationships:** parents (biological, adopted, step, foster), siblings (derived from shared parents), spouses with status (married, partner, engaged, separated, divorced, widowed), children (derived).
- **Canvas:** drag to pan, scroll or pinch to zoom, **Fit** button. Background pattern (dots, grid, lines, plain) with custom paper and pattern colours.
- **Search:** matches names, other names and biography; results show birth year and parents to tell same-name people apart.
- **Focus view:** show only a person's ancestors, descendants, both, or their whole connected family.
- **How are we related?** Blood relationships through a shared ancestor, plus spouses.
- **Undo / redo:** last 40 steps (buttons, or Ctrl/Cmd+Z and Ctrl/Cmd+Y when not typing).

## Data model

One flat record per person; children and siblings are derived, never stored.

```json
{
  "id": "k3j9x0a", "first": "", "middle": "", "last": "", "other": "",
  "gender": "", "color": "", "photo": "",
  "born": "", "btime": "", "bplace": "", "died": "", "dtime": "", "dplace": "", "dec": false,
  "dates": "Married: 1974-06-02", "bio": "", "src": "",
  "parents": ["id"], "pt": { "parentId": "adopted" },
  "spouses": ["id"], "ms": { "spouseId": "married" }
}
```

## Import / export

- **Export all**, or select a person and **Export branch** (descendants, ancestors, or whole connected family). The JSON appears in a dialog to copy.
- **Import** accepts pasted JSON, a file, or GEDCOM. Imports merge by `id`: matching people are updated, others are added. Imports can be undone.
- **GEDCOM 7.0:** in the export dialog, press **JSON → GEDCOM 7**. Names, sex, birth / death (date, time, place), notes, events, families (marriage / divorce / engagement), parent type and photos (embedded as data URIs) are carried. Not carried: card colours and a third or later parent. Sources are exported as plain notes. Gender other than female / male is exported as `X`. On import, only embedded photos are read.

## Limits

- Parent-to-child layout is simple (generation rows, ordered by parents), so very large or heavily intermarried trees may get crowded or have crossing lines.
- "How are we related?" does not cover in-laws or distinguish half from full siblings.