<p align="center">
  <img src="https://raw.githubusercontent.com/EconViz/.github/main/profile/assets/banner.svg" alt="EconViz" width="480">
</p>

<p align="center">
  Open-source Python packages for economics diagrams.
</p>

<p align="center">
  <a href="https://econ-viz.org">Documentation</a> ·
  <a href="mailto:contact@econ-viz.org">contact@econ-viz.org</a>
</p>

## Packages

EconViz is made of six packages. Two draw economics diagrams, and four are general-purpose building blocks they share.

### Economics

| Repository | Description |
|---|---|
| [**utility&#8209;viz**](https://github.com/EconViz/utility-viz) | Publication-quality microeconomics diagrams: indifference curves, budget constraints and consumer equilibria, with a built-in solver and PNG / PDF / SVG / TikZ export. Version 2.0 is in beta. |
| [**principle&#8209;viz**](https://github.com/EconViz/principle-viz) | Market analysis and diagrams for Principles of Economics: supply and demand, taxes, price controls and welfare decomposition (CS, PS, DWL). |

### Foundation

| Repository | Description |
|---|---|
| [**mosaickit**](https://github.com/EconViz/mosaickit) | A domain-neutral toolkit for assembling two-dimensional diagrams from scenes, layers, styles and renderers. |
| [**bezierkit**](https://github.com/EconViz/bezierkit) | A small toolkit for constructing, analyzing and exporting Bézier curves, with SVG and TikZ output. |
| [**glyphkit**](https://github.com/EconViz/glyphkit) | A domain-neutral typography and glyph geometry engine: font loading, text and math layout. |
| [**kinetickit**](https://github.com/EconViz/kinetickit) | A domain-neutral engine for precise 2D and 3D animation. |

### Project

| Repository | Description |
|---|---|
| [**econ&#8209;viz&#8209;docs**](https://github.com/EconViz/econ-viz-docs) | Source of the [econ-viz.org](https://econ-viz.org) documentation site, built with MkDocs Material. |
| [**agora**](https://github.com/EconViz/agora) | Reference manuals for the EconViz packages. |

## Quick Start

```bash
uv add --prerelease allow utility-viz
```

```python
from utility_viz import Canvas, levels, solve
from utility_viz.models import CobbDouglas

model = CobbDouglas(alpha=0.5, beta=0.5)
eq = solve(model, px=2.0, py=3.0, income=30.0)

cvs = Canvas(x_max=20, y_max=15, x_label="x", y_label="y")
cvs.add_utility(model, levels=levels.around(eq.utility, n=5))
cvs.add_budget(2.0, 3.0, 30.0, fill=True)
cvs.add_equilibrium(eq, show_ray=True)
cvs.save("cobb_douglas.png")
```

### utility-viz

| Consumer equilibrium | Price effect | Edgeworth box | Leontief |
|---|---|---|---|
| <img src="https://raw.githubusercontent.com/EconViz/econ-viz-docs/main/docs/assets/utility-viz/agora/canvas/show_save.svg" alt="Consumer equilibrium" width="220"> | <img src="https://raw.githubusercontent.com/EconViz/econ-viz-docs/main/docs/assets/utility-viz/agora/consumer/hicks.svg" alt="Price effect" width="220"> | <img src="https://raw.githubusercontent.com/EconViz/econ-viz-docs/main/docs/assets/utility-viz/agora/consumer/edgeworth.svg" alt="Edgeworth box" width="220"> | <img src="https://raw.githubusercontent.com/EconViz/econ-viz-docs/main/docs/assets/utility-viz/agora/models/leontief.svg" alt="Leontief" width="220"> |

### principle-viz

| Demand shift | Tax welfare | Price ceiling | Tariff |
|---|---|---|---|
| <img src="https://raw.githubusercontent.com/EconViz/econ-viz-docs/main/docs/assets/principle-viz/agora/markets/shift_demand_increase.svg" alt="Demand shift" width="220"> | <img src="https://raw.githubusercontent.com/EconViz/econ-viz-docs/main/docs/assets/principle-viz/agora/taxes/welfare.svg" alt="Tax welfare" width="220"> | <img src="https://raw.githubusercontent.com/EconViz/econ-viz-docs/main/docs/assets/principle-viz/agora/controls/ceiling_welfare.svg" alt="Price ceiling" width="220"> | <img src="https://raw.githubusercontent.com/EconViz/econ-viz-docs/main/docs/assets/principle-viz/agora/trade/tariff.svg" alt="Tariff" width="220"> |

Coming from `econ-viz` 1.x? See the [migration guide](https://econ-viz.org/utility-viz/migrating/).

## Contributing

Issues and pull requests are welcome. See [CONTRIBUTING.md](https://github.com/EconViz/.github/blob/main/CONTRIBUTING.md) to get started.

## Contact

Questions, collaboration, or teaching use? Email [contact@econ-viz.org](mailto:contact@econ-viz.org).
