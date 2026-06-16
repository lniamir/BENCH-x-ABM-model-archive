# Adding a New Model to the BENCH Archive

This document covers the full process for adding a new model to the family — from the diagram through to the live website and downloadable release.

---

## 1. Update the family diagram

Open `docs/BENCH_Family_Models_diagram.drawio` in [draw.io](https://app.diagrams.net/).

Add the new model as a node in the diagram following the same style as existing nodes. Once finished:

1. Note the **x, y, width, height** of the new node's shape — you can read these from the panel on the right in draw.io when the shape is selected.
2. Export: **File > Export As > SVG**, transparent background, save as `docs/BENCH_Family_Models_diagram.svg` (overwrite the existing file).

---

## 2. Add the model code

Add the model's folder under `models/`:

```
models/
└── BENCH_NewModel/
    ├── README.md       # model-level documentation
    └── ...             # scripts, inputs, etc.
```

---

## 3. Update the website (`docs/index.html`)

Two things to add:

### A. Hotspot over the new diagram node

Calculate the hotspot position as a percentage of the SVG canvas (978 × 911 px):

```
left%   = x / 978 × 100
top%    = y / 911 × 100
width%  = width / 978 × 100
height% = height / 911 × 100
```

Add a new `<div class="hotspot">` inside `<div class="diagram-wrap">`, following the same pattern as existing nodes:

```html
<div class="hotspot" data-model="newmodel"
     style="left:XX%; top:XX%; width:XX%; height:XX%">
  <span class="hotspot-label">BENCH_NewModel</span>
</div>
```

### B. Model data in the JavaScript block

Add an entry to `MODEL_DATA` in the `<script>` section:

```js
newmodel: {
  title: 'BENCH_NewModel',
  lang: 'Python (Mesa)',   // or NetLogo, etc.
  desc: 'One sentence description of what the model does.',
  paper: {
    label: 'Full paper title (year)',
    url: 'https://doi.org/...',   // set to null if not yet published
  },
  download: RELEASE_BASE + 'BENCH_NewModel_clean.zip',
},
```

### C. Add a row to the table

Add a row to the `<tbody>` of the "All models" table at the bottom of the page:

```html
<tr>
  <td><strong>BENCH_NewModel</strong></td>
  <td>Python (Mesa)</td>
  <td><a href="https://doi.org/..." target="_blank">Paper title (year)</a></td>
  <td><a href="https://github.com/lniamir/bench-model-archive/releases/download/v1.0/BENCH_NewModel_clean.zip" target="_blank">Download</a></td>
</tr>
```

---

## 4. Update the README

Add a row to the table in `README.md`:

```markdown
| [**BENCH_NewModel**](./models/BENCH_NewModel) | Python (Mesa) | [Paper title (year)](https://doi.org/...) |
```

---

## 5. Create the release zip

Prepare a clean zip of just the model folder — no `.venv`, no cache, no output files. Name it `BENCH_NewModel_clean.zip`.

To add it to the existing GitHub release:
1. Go to the repository on GitHub > **Releases**
2. Click the pencil (edit) icon on release `v1.0`
3. Drag the new zip into the **Assets** section
4. Click **Update release**

> If the new model represents a significant milestone, consider creating a new release (e.g. `v2.0`) instead of updating `v1.0`. In that case, update the `RELEASE_BASE` constant in `docs/index.html` and all table download links to point to the new tag.

---

## 6. Commit and push

```bash
git add docs/BENCH_Family_Models_diagram.svg docs/index.html README.md models/BENCH_NewModel/
git commit -m "Add BENCH_NewModel to archive"
git push
```

GitHub Pages will redeploy automatically within a minute or two.
