[![codecov](https://codecov.io/gh/sarajahedazad/ME700-HW3/graph/badge.svg?token=DRF09dG4CR)](https://codecov.io/gh/sarajahedazad/ME700-HW3)
[![Run Tests](https://github.com/sarajahedazad/ME700-HW3/actions/workflows/tests.yml/badge.svg)](https://github.com/sarajahedazad/ME700-HW3/actions/workflows/tests.yml)


# ME700-HW3

### Conda environment, install, and testing

Note: To be completed.

```bash
conda create --name finite-element-analysis-env python=3.12.9
```

```bash
conda activate finite-element-analysis-env
```

```bash
python --version
```

```bash
pip install --upgrade pip setuptools wheel
```

```bash
pip install -e .
```

```bash
pytest -v --cov=finiteelementanalysis --cov-report term-missing
```

