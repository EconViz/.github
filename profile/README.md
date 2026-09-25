<p align="center">
  <img src="https://raw.githubusercontent.com/EconViz/econ-viz-docs/main/docs/assets/banner.svg" alt="Econ-Viz" width="480">
</p>

<p align="center">
  Open-source Python tools for drawing publication-quality economics diagrams.
</p>

<p align="center">
  <a href="https://econ-viz.org"><img alt="Docs" src="https://img.shields.io/badge/docs-econ--viz.org-181818?style=flat-square&color=181818&labelColor=f3f3f3"></a>
  <a href="https://pypi.org/project/econ-viz/"><img alt="econ-viz on PyPI" src="https://img.shields.io/pypi/v/econ-viz?style=flat-square&color=181818&labelColor=f3f3f3&label=econ-viz"></a>
  <a href="https://pypi.org/project/principle-econ/"><img alt="principle-econ on PyPI" src="https://img.shields.io/pypi/v/principle-econ?style=flat-square&color=181818&labelColor=f3f3f3&label=principle-econ"></a>
</p>

## Projects

| Repository | Description |
|---|---|
| [**econ-viz**](https://github.com/EconViz/econ-viz) | Indifference curves, budget constraints, and consumer equilibria — with a built-in solver, closed-form demand in TeX, Slutsky tools, and PNG / PDF / SVG / TikZ export. |
| [**principle-econ**](https://github.com/EconViz/principle-econ) | Principles-level market analysis: linear demand and supply, taxes, price controls, and welfare decomposition (CS, PS, DWL). |
| [**econ-viz-docs**](https://github.com/EconViz/econ-viz-docs) | Documentation source for [econ-viz.org](https://econ-viz.org), built with MkDocs Material. |

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
