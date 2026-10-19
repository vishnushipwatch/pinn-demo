# PINN Vessel Performance Demo

An interactive, browser-based demo for exploring how a physics-informed vessel performance model responds to time progression and weather conditions.

The interface renders a 3D surface for **speed**, **draft**, and estimated **power consumption**, alongside synthetic observations and a compass-style weather control panel. It is designed as a lightweight visualization prototype: there is no build step, backend, or model download required.

## Features

- Interactive Plotly 3D surface of the adapted vessel performance model
- Synthetic observation points that appear and fade as the timeline advances
- Timeline from baseline through three months of model progression
- Wind direction and wave direction controls from 0° to 359°
- Beaufort wind-force control from 0 to 6 BF
- Weather exposure factor applied to the estimated consumption surface
- Responsive layout for desktop and mobile screens
- Reduced-motion support through `prefers-reduced-motion`

## Quick start

The demo is a single HTML file and can be opened directly in a modern browser:

1. Clone or download this repository.
2. Open [`pinn_interactive.html`](./pinn_interactive.html) in a browser.

For a local HTTP server, use any static file server. For example, with Python:

```powershell
python -m http.server 8000
```

Then open <http://localhost:8000/pinn_interactive.html>.

> The page loads Plotly and web fonts from CDNs, so the first load requires internet access. A local server is recommended when testing browser behavior rather than opening the file directly.

## Using the demo

- **Rotate the chart:** drag inside the 3D plot.
- **Zoom the chart:** use the mouse wheel or trackpad gesture.
- **Advance time:** move the timeline slider from baseline to three months. The slider advances in quarter-month increments.
- **Change wind and waves:** adjust the direction sliders to rotate the corresponding bearings on the compass.
- **Increase weather severity:** move the Beaufort slider. Higher values increase wind and wave exposure effects and animate the wind gusts more quickly.
- **Inspect a point:** hover over the surface or observation markers to see speed, draft, and consumption values.

## Model notes

The demo intentionally uses deterministic, synthetic data so that it is reproducible without a training dataset or inference service.

The displayed surface is calculated from a power-law relationship:

```text
P = k · draft^(2/3) · speed^n · weather_factor
```

Where:

- `P` is estimated consumption in kW.
- `n` transitions from 2 at baseline to 4 at the end of the timeline.
- `k` is calibrated against a reference point of 12 kn, 9 m draft, and 4,520 kW.
- `weather_factor` depends on wind direction, wave direction, and Beaufort force.

The observations and progression are illustrative. They should not be interpreted as operational vessel advice, validated fuel-consumption estimates, or a substitute for a trained and calibrated production model.

## Project structure

```text
pinn-demo/
├── pinn_interactive.html  # Complete UI, styles, synthetic data, and model logic
└── README.md               # Project documentation
```

## Dependencies

There are no local package dependencies or build tools. The page uses:

- [Plotly.js 2.35.2](https://cdn.plot.ly/plotly-2.35.2.min.js) for the 3D visualization
- [Manrope](https://fonts.google.com/specimen/Manrope) and [DM Mono](https://fonts.google.com/specimen/DM+Mono) from Google Fonts

## Development

Edit `pinn_interactive.html` directly, then refresh the browser to see changes. The main areas are:

- CSS in the `<style>` block for layout and visual styling
- Synthetic data and model functions in the `<script>` block
- `render()` for recalculating Plotly traces and updating the controls

Because the demo is self-contained, no compilation, bundling, or test runner is currently required.

## License

No license has been specified for this repository yet. Add a license before distributing or reusing the code outside the project.
