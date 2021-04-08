---
layout: post
title:  "Using Jupyter Notebooks in Visual Studio Code with venv"
date:   2021-04-07 12:00:00 -0800
categories: python
---

I had my Jupyter notebook open in Visual Studio Code and I just installed ```matplotlib```. When I tried to run ```import matplotlib.pyplot as plt``` in one of the cells, I got the ModuleNotFoundError.

![ModuleNotFoundError](https://carloandaya.s3-us-west-2.amazonaws.com/assets/images/posts/20210407_02_modulenotfounderror.png)

## Lead Up

Before we dig in to using the virtual environment with a Jupyter notebook, let's go through the steps of creating a virtual environment and installing all the required modules.

In a new VS Code project, open a terminal and create a new virtual environment with

```python -m venv .venv```

Activate the new virtual environment

```.venv\Scripts\activate```

Create a new file with an extension of ```.ipynb``` to activate the Jupyter extension.

Install matplotlib with

```pip install matplotlib```

## The Problem

As mentioned at the beginning, importing matplotlib in a cell results in a ModuleNotFoundError.

## The Solution

Start by installing ipykernel.

```pip install ipykernel```

Create a new kernel for Jupyter to use (replace *projectname* with the name of your project).

```python -m ipykernel install --user --name=projectname```

Restart Visual Studio Code to clear the cache.

Switch to the kernel for the project.

![Kernel Switch](https://carloandaya.s3-us-west-2.amazonaws.com/assets/images/posts/20210407_03_kernelswitch.png)

You can now import ```matplotlib``` and check the version.

![Get Version](https://carloandaya.s3-us-west-2.amazonaws.com/assets/images/posts/20210407_04_matplotlibversion.png)
