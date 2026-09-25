# Contributing to EconViz

Thanks for your interest in contributing! This is the default guide for all repositories in the EconViz organization. A repository may ship its own `CONTRIBUTING.md` with more specific instructions — if it does, that one takes precedence.

## Before you start

- Check the **[econ-viz Roadmap](https://github.com/orgs/EconViz/projects/1)** to see what is already planned or in progress.
- Search existing [issues](https://github.com/issues?q=org%3AEconViz) before opening a new one.
- For larger changes, open an issue first so we can agree on the approach before you write code.

## Reporting bugs

Open an issue with:

- The package and version (`pip show econ-viz`)
- Your Python version and OS
- A minimal code snippet that reproduces the problem
- What you expected, and what happened instead (attach the output figure if relevant)

## Suggesting features

Open an issue describing the economic concept or use case, ideally with a reference (textbook, paper, lecture notes) or a sketch of the diagram you want to produce.

## Pull requests

1. Fork the repository and create a branch from `main`.
2. Install the development environment:

   ```bash
   pip install poetry
   poetry install --with dev
   ```

3. Make your change and add tests for any new behaviour.
4. Run the test suite: `poetry run pytest`
5. Open a pull request against `main` and describe what changed and why.

## Code style

- Python 3.12+
- Follow the existing style of the repository
- Keep public APIs documented with docstrings

## License

By contributing, you agree that your contributions will be released under the license of the repository you are contributing to (MIT for all current EconViz projects).
