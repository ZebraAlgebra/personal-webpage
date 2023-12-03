+++
title = "Project: flusim"
description = "A simulation package for a simple flu problem"
slug = "projects-omsa-01"
date = 2023-12-03
+++

![](../flusim.png)

> Link to the [GitHub Repo](https://github.com/ZebraAlgebra/flusim)

> Link to the [published package](https://test.pypi.org/project/flusim/0.0.1/)

> Link to a [demo Jupyter notebook](https://htmlpreview.github.io/?https://github.com/ZebraAlgebra/flusim/blob/main/notebooks/package_demo.html)

## Overview

This is a school project in the course [ISYE6644 Simulation](https://omscs.gatech.edu/isye-6644-simulation-and-modeling-engineering-and-science) that I worked on my own.

The problem is to simulate, calculate some statistics of a small-scale, simple flu-spread problem.

## Problem Statement

The **flu-spread problem** has the following very simple assumptions:

1. There are `n` people in a closed system, with no people being sick before day `0`, and `1` people sick in day `1`.
2. Every people once sick, will recover in exactly `l` days.
3. Each day, every healthy person has a probability `p` of getting infected by an ill person, and these events are independent.

The problem is to determine the expectation and variance of the number of ill people each day, as well as the same statistics for the date of flu end.

## Skills Obtained

1. Python:
   - OOP stuffs (Pythonic inheritance, `dataclass` syntaxes)
   - Package development (package structures, typing, validation, package publishing ecosystem)
2. Math:
   - Basic Markov chain analysis (state space estimates, analysis on the statistics of time till absorption)
   - Output analysis (confidence intervals)

## Things I did

1. On the mathematical side, I recognized that these statistics can be calculated via an associated Markov chain.
   - In this sense, these statistics can be numerically solved via matrix equations.
     - For the statistics of each day, the number is a linear or quadratic function on the entries of a power of the transition matrix (of the associated Markov chain).
     - For flu-end dates, the expectation and variance satisfies a recurrence relation, derived via a standard first-step analysis.
   - The problem is that since the assocaited Markov chain has a large state space (almost of size `n ** d`), other simulation methods are needed.
2. On the simulation side, I wrote codes that can simulate the associated Markov chain, and used it to find the approximate statistics, confidence intervals.
   - To simulate and see the status of every person in the system at each day, a more resource-heavy method must be used, so I implemented this functionality as a single period simulation.
   - If the number of people being sick each day is our only focus, a faster method can be used, so I implemented this functionality as a multiple period simulation.
3. On the visualization side, I wrote two simple visualization widget using [Bokeh](https://bokeh.org/) to visualize simulation results.
4. Combining all these functionalities, I wrote a Python package and published it on test PyPI.
