# Family Tree

A small simple personal family tree app.

## Run

Open `index.html` in a browser, or host it anywhere static (see Deploy). Data is saved automatically in the browser's `localStorage` (key `famtree.v1`), so it is per browser and per device. Use **Export all** regularly as a backup.

## Deploy (GitHub Pages)

1. Create a repo and add `index.html` and `README.md` at the repo root.
2. Repo **Settings → Pages → Build and deployment**: Source **Deploy from a branch**, branch `main`, folder `/ (root)`.
3. After a minute it is live at `https://<user>.github.io/<repo>/`.

Notes:
- There is no build step and all paths are relative; only Google Fonts is loaded from the network (it falls back to system fonts offline).
- Your data lives in each visitor's browser, not in the repo. `localStorage` is shared by every page on the same `<user>.github.io` origin, so it is stored under the key `famtree.v1`.
- A public Pages site is public to everyone. Do not commit exported family data to a public repo; keep exports somewhere private.

## Features

- **People:** first / middle / last name, other names, gender (free text with suggestions), born / died (standardized as year, year-month or full date, with optional times and places), deceased flag, age, significant dates, biography, sources, photo, card colour (each colour has a matching border).
- **Relationships:** parents (biological, adopted, step, foster), siblings (derived from shared parents), spouses with status (married, partner, engaged, separated, divorced, widowed), children (derived).
- **Linking on the canvas:** drag the ⊕ on a card onto another card and choose parent, child, spouse or sibling (choices that would create a loop are disabled).
- **Canvas:** drag to pan, scroll or pinch to zoom, **Fit** button. Siblings are ordered oldest to youngest, left to right. Background pattern (dots, grid, lines, plain) with custom paper and pattern colours, under the ⚙ menu.
- **Search:** matches names, other names and biography; results show birth year and parents to tell same-name people apart.
- **Focus view:** the *Focus* dropdown (bottom left) shows only the selected person's ancestors, descendants, both, or whole family, and follows whoever you select.
- **Views:** the view selector in the header switches between *Tree*, *Life spans* (a bar per person, birth to death, packed into rows; people without a birth year are not shown) and *Events* (a single axis with markers for births, deaths, marriages and other dated notes). Marriages and other events come from each person's *Significant dates* lines written as `Label: date` (for example `Married: 1974-06-02`); labels containing “marri” or “wedd” count as marriages, and identical entries on two people are merged into one marker.
- **How are we related?** Blood relationships through a shared ancestor, plus spouses.
- **Undo / redo:** last 40 steps (buttons, or Ctrl/Cmd+Z and Ctrl/Cmd+Y when not typing).

## Data model

One flat record per person; children and siblings are derived, never stored.

```json
{
  "id": "k3j9x0a", "first": "", "middle": "", "last": "", "other": "",
  "gender": "", "color": "", "photo": "",
  "born": "1950-03", "btime": "", "bplace": "", "died": "", "dtime": "", "dplace": "", "dec": false,
  "dates": "Married: 1974-06-02", "bio": "", "src": "",
  "u": 0,
  "parents": ["id"], "pt": { "parentId": "adopted" },
  "spouses": ["id"], "ms": { "spouseId": "married" }
}
```

## Import / export

- **Export all**, or select a person and **Export branch** (descendants, ancestors, or whole connected family). The JSON appears in a dialog to copy.
- **Import** accepts pasted JSON, a file, or GEDCOM. Import doubles as **sync**: every edit stamps the person with an update time (`u`), and importing merges by `id`: new people are added, a matching person is replaced only if the incoming record is newer (otherwise yours is kept), and relationships to people outside the file are preserved, so importing a branch never cuts it off from the rest of your tree. A summary shows how many were added, updated, kept or unchanged. Imports can be undone. Limitations: deletions do not sync (import never deletes), a removed relationship can reappear if the other person's record is newer, and GEDCOM or older exports have no timestamps (incoming wins ties).
- **GEDCOM 7.0:** in the export dialog, press **JSON → GEDCOM 7**. Names, sex, birth / death (date, time, place), notes, events, families (marriage / divorce / engagement), parent type and photos (embedded as data URIs) are carried. Not carried: card colours and a third or later parent. Sources are exported as plain notes. Gender other than female / male is exported as `X`. On import, only embedded photos are read.

## Limits

- Dates are stored as `YYYY`, `YYYY-MM` or `YYYY-MM-DD`; other formats (including GEDCOM phrases such as `ABT 1900`) are reduced to the year on import.
- Parent-to-child layout is simple (generation rows; children are centred under their parents unless a neighbour is in the way), so very large or heavily intermarried trees may get crowded or have crossing lines.
- "How are we related?" does not cover in-laws or distinguish half from full siblings.