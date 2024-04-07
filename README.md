# fasttrees
| __Packages and Releases__ | ![PyPI - Version](https://img.shields.io/pypi/v/fasttrees)  |
| :--- | :--- |
| __Build Status__ | [![Upload Python Package to TestPyPI](https://github.com/fasttrees/fasttrees/actions/workflows/python-publish-testpypi.yml/badge.svg)](https://github.com/fasttrees/fasttrees/actions/workflows/python-publish-testpypi.yml) [![Upload Python Package to PyPI](https://github.com/fasttrees/fasttrees/actions/workflows/python-publish.yml/badge.svg)](https://github.com/fasttrees/fasttrees/actions/workflows/python-publish.yml) [![Python package](https://github.com/fasttrees/fasttrees/actions/workflows/python-package.yml/badge.svg)](https://github.com/fasttrees/fasttrees/actions/workflows/python-package.yml) ![pyling: workflow](https://github.com/fasttrees/fasttrees/actions/workflows/pylint.yml/badge.svg) |
| __Test Coverage__ | [![codecov](https://codecov.io/github/fasttrees/fasttrees/graph/badge.svg?token=XCJQ3NXKVT)](https://codecov.io/github/fasttrees/fasttrees) |
| __Other Information__ | [![Downloads](https://static.pepy.tech/badge/fasttrees)](https://pepy.tech/project/fasttrees) ![PyPI - Python Version](https://img.shields.io/pypi/pyversions/fasttrees) [![linting: pylint](https://img.shields.io/badge/linting-pylint-yellowgreen)](https://github.com/pylint-dev/pylint) |


A fast-and-frugal-tree classifier based on Python's scikit learn.

Fast-and-frugal trees are classification trees that are especially useful for making decisions under uncertainty. 
Due their simplicity and transparency they are very robust against noise and errors in data.
They are one of the heuristics proposed by Gerd Gigerenzer in [Fast and Frugal Heuristics in Medical Decision](library.mpib-berlin.mpg.de/ft/gg/GG_Fast_2005.pdf). This particular implementation is based on on the R package [FFTrees](https://cran.r-project.org/web/packages/FFTrees/index.html), developed by Phillips, Neth, Woike and Grassmaier.

## Install
You can install fasttrees using
```
pip install fasttrees
```
