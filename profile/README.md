<p align="center">
  <img src="https://raw.githubusercontent.com/EconViz/econ-viz-docs/main/docs/assets/banner.svg" alt="Econ-Viz" width="480">
</p>

<p align="center">
  Open-source Python tools for drawing publication-quality economics diagrams.
</p>

## Projects

| Repository | Description |
|---|---|
| [**econ&#8209;viz**](https://github.com/EconViz/econ-viz) | Indifference curves, budget constraints, and consumer equilibria — with a built-in solver, closed-form demand in TeX, Slutsky tools, and PNG / PDF / SVG / TikZ export. |
| [**principle&#8209;econ**](https://github.com/EconViz/principle-econ) | Principles-level market analysis: linear demand and supply, taxes, price controls, and welfare decomposition (CS, PS, DWL). |
| [**econ&#8209;viz&#8209;docs**](https://github.com/EconViz/econ-viz-docs) | Documentation source for [econ-viz.org](https://econ-viz.org), built with MkDocs Material. |

## Quick Start

```bash
pip install econ-viz
```

```python
from econ_viz import Canvas, levels, solve
from econ_viz.models import CobbDouglas

model = CobbDouglas(alpha=0.5, beta=0.5)
eq    = solve(model, px=2.0, py=3.0, income=30.0)

cvs = Canvas(x_max=20, y_max=15, x_label="x", y_label="y")
cvs.add_utility(model, levels=levels.around(eq.utility, n=5))
cvs.add_budget(2.0, 3.0, 30.0, fill=True)
cvs.add_equilibrium(eq, show_ray=True)
cvs.save("cobb_douglas.png")
```

<p align="center">
  <img src="https://raw.githubusercontent.com/EconViz/econ-viz/a8423043789ee7dba19b2d71fa6cc5071601181a/cobb_douglas_eq.png" alt="Cobb-Douglas indifference map with budget line and equilibrium point" width="520">
</p>

## Contributing

Issues and pull requests are welcome. See [CONTRIBUTING.md](https://github.com/EconViz/econ-viz/blob/main/CONTRIBUTING.md) in the main repository to get started.
