---
title: Dynamic 'run magic' in Synapse Spark Notebooks
tags:
  - Azure
  - "#Data"
  - spark
  - synapse
---
# Introduction

In a collaborative environment, it's likely that you'll need to work with other notebooks in the same environment. For example, if writing a utility function that can be used across numerous notebooks, it is not always possible or appropriate to push those new notebooks to main prior calling that notebook elsewhere.
As such, it's essential to set a property on the notebook that allows for unpublished notebooks to be referenced in parent notebooks. This will be the case when

a) Creating a new notebook that will be used by other notebooks.
b) updating a notebook that will be used by other notebooks.

The property can be set on each notebook.

![[Pasted image 20240108171710.png]]

> [!warning] Warning - property must be set on both caller and called notebook
> This is a gotcha that catches a few people out. The property must be assigned on both the parent and child notebooks. If set on parent and child notebooks, then even notebooks that have not yet been published can be references.
# Methods Calling other notebooks

There are two ways to call other notebooks.

## Static
If the same notebook will be called each time, then simply hard code the name of the notebook using the run magic command as below.
```python
%run /folder/subfolder/notebook_name
```
Note that the location of the notebook to be called is not enclosed in strings.

## Dynamic
If the notebook to be called is established during notebook execution (such as from a config database, or based on other logic), then use the 
```python
notebook_loc='/folder/subfolder/notebook_name'
mssparkutils.notebook.run(notebook_loc)
```

the mssparkutils is sadly not a 'it just works' solution to this problem. Occasionally there are issues where we face errors where the problem is not obvious. I faced an issue with a called notebook that had a sql cell. After refactoring the sql cell, I ensured that each cell contained only one SQL statement, and my issue was resolved.









