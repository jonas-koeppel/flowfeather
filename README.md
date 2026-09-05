# Feather Flow

Feather Flow is a lightweight browser app for exploring flow cytometry data. It runs as a static HTML app, keeps analysis local in the browser, and focuses on fast visual gating, sample comparison, and polished exports.

## Features

- Import `.fcs` and `.csv` files, including drag-and-drop loading into the empty plot workspace or Files panel.
- Apply reversible manual fluorescence compensation with a detector-aware spillover matrix.
- View samples as scatter overlays, scatter grids, histogram overlays, and histogram offsets, with overlay histograms normalized by peak or shown as event counts.
- Draw rectangle, ellipse, polygon, and histogram segment gates.
- Build hierarchical populations, including sample-specific gates and apply-to-sample or apply-to-all workflows.
- Move and edit gates, labels, axes, sample colors, and grid column layout interactively.
- Export publication-style plots as PDF or PNG.
- Export gated populations and configurable statistics as CSV.
- Save and reload workspace JSON files with samples, axes, gates, and view state.
- Use light or dark mode, with matching plot/export options.

## Getting Started

Feather Flow has no build step. Serve the folder with any local static server:

```bash
python3 -m http.server 8765
```

Then open:

```text
http://127.0.0.1:8765
```

You can also open `index.html` directly in a browser for basic local file loading. The bundled example data works best through a local server because browsers restrict some file access from `file://` pages.

PDF export uses jsPDF from the unpkg CDN, so PDF export requires an internet connection unless that dependency is vendored locally.

## Data Formats

### FCS

Feather Flow includes a small in-browser FCS parser intended for common numeric FCS files. Channels are read from the file metadata and made available as plotting parameters.

### CSV

CSV files should use the first row for parameter names. Numeric columns are imported as flow parameters.

## Basic Workflow

1. Add one or more `.fcs` or `.csv` files.
2. Choose X/Y parameters and a plot type.
3. Open **Compensation** in the Data panel if the acquisition needs spillover correction.
4. Adjust linear/log scales and axis ranges as needed.
5. Draw gates from the Gates panel.
6. Double-click a gate in the plot or hierarchy to select that population.
7. Use exports for figures, statistics, or event-level population data.
8. Save a workspace JSON if you want to return to the session later.

## Compensation

The Compensation control stays collapsed in the Data panel until needed. Open it, select two or more fluorescence detectors, and enter spillover percentages. Rows are source channels and columns are the detectors in which that source signal was measured; diagonal values remain fixed at 100%.

Applying the matrix updates plots, gates, statistics, and exports. Raw event values are retained in memory, so compensation can be edited or disabled without reloading files. The raw values and compensation settings are also stored safely in workspace files, preventing double compensation when a workspace is reopened.

`compensation_test_data/` contains a small public set of unstained and singly stained FCS controls for exercising this workflow.

## Gates

Top-level gates are drawn on All Events. Gates drawn while another population is selected become children of that population and only display when that parent population is active. In grid and offset views, gates can become sample-specific so individual samples can be adjusted independently.

Right-click a gate to apply it to a sample or all samples. Sample-specific gates are marked in the hierarchy with the sample color.

## Exports

The Export menu includes:

- Image (PDF)
- Image (PNG)
- Statistics (CSV)
- Population (CSV)

PDF export includes options for size, gates, sample labels, percent labels, all-events plotting, custom axis labels, and light/dark background. Statistics export lets you choose gates, parameters, and metrics such as events, mean, median, standard deviation, percent parent, and percent total.

## Privacy

Imported data is processed in the browser. Feather Flow does not upload samples to a server. If you use PDF export, the page loads jsPDF from `unpkg.com` unless you host that library locally.

## Development

The app is currently implemented as a static single-page app:

- `index.html` contains the UI, plotting, parsers, gates, and export logic.
- `feather-icon.svg` is the active favicon; the header uses the matching inline product mark.
- `example_data/` contains bundled example FCS files.
- `compensation_test_data/` contains openly licensed compensation control files and source notes.

Because there is no build step, refreshing the browser is enough after editing `index.html`.

## License

This project is open source under the MIT License. See [LICENSE.md](LICENSE.md).
