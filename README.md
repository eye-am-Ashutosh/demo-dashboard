# BESS Vendor Comparison Dashboard


A static, Excel-driven dashboard for comparing battery storage vendors (OEMs, EPCs, turnkey integrators)
on SoH, DC usable energy, energy at POI, RTE, losses and anything else you add as a column.
It runs entirely in the browser, so it can be hosted free on **GitHub Pages**.

## What's on the page
The dashboard is organised **by parameter**. The menu on the left lists every parameter in the workbook
(SoH, installed capacity, DC usable, POI, excess energy, RTEs…). Pick one to compare **all vendors on it**:

- **Headline numbers** at the chosen year: best vendor, weakest vendor, spread between them, and how many meet the threshold.
  With a single vendor selected: its value, change since year 0, margin vs threshold and first year it misses.
- **Trend over the years:** one line per vendor, the dashed threshold, and a dotted marker at the chosen year.
- **Ranking at the chosen year:** vendors as bars, best first, with the threshold line.
- **Year-by-year table:** values that miss the threshold in red, plus the first year each vendor misses it.

Use **Multi / Single** and the vendor buttons to choose vendors. The choice and the year slider stay the same as you
switch between parameters. Each parameter has its own link (e.g. `.../#p-poi`) you can share.

**Summaries** at the bottom of the menu:
- **All-parameter scorecard:** every parameter in one table, ★ on the best vendor per row.
- **RTE: claim vs calculated:** the vendor's claimed AC-AC RTE next to the RTE calculated from its own losses.
- **Assumptions:** container capacity, number of containers, losses, side by side.

Parameters identical for every vendor (e.g. RFQ capacity) are shown once at the top instead of in the menu.

## Excel format
See [`data/dashboard.xlsx`](data/dashboard.xlsx) for the template with dummy data.

- **One sheet per vendor.** The sheet name is the vendor name (`OEM1`, `EPC1`, …). Add as many as you like.
- **A header row containing `Year`**, then one row per year. Every other column is one parameter.
- **Assumptions go below the year table**, after a blank row: the label in the Year column and the value next to it,
  e.g. `CONTAINER CAPACITY | 5MWH`, `AC losses | 2%`.
- **Optional `THRESHOLD` sheet:** same layout. Fill only the columns that need a dashed reference line
  (e.g. SoH guarantee curve, POI = RFQ capacity, excess energy = 0).
- Percentages can be typed as `94.5` or `0.945` formatted as %.
- Sheets named `README` are ignored.

### Units and "which way is better"
These are worked out from the column header by `RULES` near the top of the script in `index.html`:

| Header contains | Unit | Better |
|---|---|---|
| SoH | % | higher |
| loss | % | lower |
| RTE / efficiency | % | higher |
| aux | kW | lower |
| charging | MWh | not ranked |
| excess / margin | MWh | higher |
| RFQ / installed | MWh | not ranked |
| capacity / usable / POI / energy | MWh | higher |

To set a unit yourself, put it in brackets at the end of the header, e.g. `AUX POWER (kW)`.
If a new parameter is ranked the wrong way, add a line to `RULES`.

## Publish on GitHub Pages
1. Create a repo and upload `index.html`, the `data` folder and this README.
2. Repo → **Settings → Pages** → Source: *Deploy from a branch* → `main` / `root` → Save.
3. Open `https://<your-user>.github.io/<repo>/`.

To update what everyone sees, replace `data/dashboard.xlsx`. Files opened with the **Upload** button stay in
the viewer's browser and are never sent anywhere. Note that in a public repo anyone can download `data/dashboard.xlsx`,
so keep confidential data out of it.

## Run locally
```bash
python3 -m http.server 8765
```
Then open http://localhost:8765. Opening `index.html` directly also works; it falls back to a built-in copy of the sample data
