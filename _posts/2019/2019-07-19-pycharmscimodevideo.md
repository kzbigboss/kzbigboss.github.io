---
layout: post
title: Video Notes - Scientific Mode in PyCharm
categories: learning
---

For data science projects, the IDE I am most comfortable with is R Studio.  Since Python has become my primary language lately, I want to consolidate my data analysis into PyCharm.  PyCharm published a webinar titled, "Effective Data Science with PyCharm" [(YouTube link)](https://youtu.be/46RjXawJQgg).  This post contains the notes I took while watching this webinar.

---

## Using PyCharm as a Data Science IDE
* Toggle on 'Scientific Mode' in the View menu.
  * You can bind the Scientific Mode to a keyboard shortcut.
* Defining "cells" in a script
  * Cells represent groupings of code that can run together.  Basically a code cell from Jupyter.
  * Define a cell by adding `#%%` at the top:

    ``` python
    #%% cell one, import
    import pandas as pd
    import datetime

    #%% cell two, run
    print("hello world")
    ```
  * **Ctrl+Enter** executes a single cell
  * Best to use `path()` and `.joinpath()` methods from `Pathlib` to avoid hard coding OS specific paths.

* **Ctrl+Alt+M** copy/pastes highlighted code into it's own method.
  * It provides the `def blahrah():` line, includes your code in the code block, then provides a `blahrah()` immediately where you code was.
  * Makes it easy to refactor your code into functions that can leverage for parameters instead of hard coding (think **filters** and **group_by**)

## Jupyter Support in PyCharm
  * Create a new notebook in PyCharm
  * Same code cell syntax to define Jupyter boxes: `#%%`
  * Question mark in the toolbar provides basic Jupyter actions:
    * Run Cell, Debug Cell, Add Cell Above/Below, etc...
  * DataGrip basically lives in the Database pane
    * Able to draw diagrams to explain schema of a datebase.
      * Dependent on how PK/FK are setup.
    * Can quickly show a sample of an object.
  * Predictive text available for helping with joins based on schema's PK/FK setup.
