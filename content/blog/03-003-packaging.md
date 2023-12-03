+++
title = "Packaging and Publishing Python Projects <🐍 Python>"
description = "Post on Python's Packaging"
slug = "03-003"
date = 2023-12-03
draft = false
+++

In this post, I will be writing about the steps of publishing a package, as well as what resources helped me the most.

> Disclaimer: Since my package is a relatively simple and small one, this tutorial does not present the best practices on the process of packaging, publishing a project, but rather, a basic way of doing so.

## Background

I recently worked on a school project in the course [ISYE6644 Simulation](https://omscs.gatech.edu/isye-6644-simulation-and-modeling-engineering-and-science) and wrote some Python code for it.

Throughout the process I was thinking: well, maybe if I can build, publish this as a Python package, and that people can run `pip install` to install it, wouldn't that be pretty fun?

After going through some tutorials and going through some documentations (which is not an entirely trivial process), I finally published the [package](https://test.pypi.org/project/flusim/0.0.1/) on [testpypi](https://test.pypi.org/).

![](../flusim.png)

The amazing tutorial that helped me the most is the [Youtube video by ArjanCodes](https://www.youtube.com/watch?v=5KEObONUkik&t=936s&pp=ygUZYXJqYW5jb2RlcyBweXRob24gcGFja2FnZQ%3D%3D), and I highly recommend checking it out.

## Overview of the Process

The process of building a project is roughly as follows:

1. You need to have the source codes, and a `setup.py` file ready.
2. Run `setuptools` on `setup.py` to build a distribution file.
3. Check the generated distribution files using `twine`.
4. Upload the package to `pypi` or `testpypi` - also using `twine`.

Pretty straightforward, isn't it?

## The Syntaxes

You can check out my project directory structure at my [Github repo](https://github.com/ZebraAlgebra/flusim).

> In order for these commands in the following sections to work, you will first need to `pip install` the packages `twine, wheel, setuptools`.

### Setup and Build

My `setup.py` file is configured as follows:

```python
from setuptools import setup, find_packages

with open("app/README.md", "r") as f:
    long_description = f.read()

setup(
    name="flusim",
    version="0.0.1",
    description="Simulation, computation, visualization of some statistics for a small-scale flu-spread problem.",
    long_description=long_description,
    long_description_content_type="text/markdown",
    package_dir={"": "app"},
    author="Samuel Wang",
    author_email="swang3068@gatech.edu",
    url="https://github.com/ZebraAlgebra/flu-   sim",
    license="MIT",
    packages=find_packages(where="app"),
)
```

After that, run the following in terminal (my terminal is git bash):

```bash
python setup.py bdist_wheel sdist
```

which will build:

1. A `build` folder containing a copy of your source codes.
2. A `dist` folder with a `.whl` (wheel) file, a `.tar.gz` file.
3. An `.egg-info` containing some other metadata (in your source code folder).

### Check

Now one can use `twine` to check if things are alright via:

```bash
twine check dist/*
```

and it will complain if some infos are not present; here, recall that `dist` is the folder built by `setuptools`.

### Publishing

To publish, first register an account on [PyPI](https://pypi.org/), or the associated testing site [test PyPI](https://test.pypi.org/).

> ArjanCodes recommends **first upload it on test PyPI**, since PyPI is the official index. Also, since recently PyPI closed its registration (due to spam registrations), one can only create accounts on test PyPI.

To publish on PyPI, run:

```bash
twine upload dist/*
```

and enter the credentials. To publish on test PyPI, modify the command to:

```bash
twine upload -r testpypi dist/*
```

> Additional details: One might need to enable 2FA on test PyPI and generate tokens to succesfully publish a package, but the test PyPI site will walk you through these.
