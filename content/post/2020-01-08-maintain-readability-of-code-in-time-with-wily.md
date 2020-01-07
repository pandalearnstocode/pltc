---
title: Maintain readability of code in time with Wily
author: Aritra Biswas
date: '2020-01-08'
slug: maintain-readability-of-code-in-time-with-wily
categories:
  - Data Science
  - Python
tags:
  - Code Quality
  - Python
  - Best practices
cover: "/img/python_img/wily.png"
draft: false
---

### Maintainability code with Wily

Wily is the tool of choice which will compute and maintain a measure of maintainability of a code in time. This can be important to check that a code is readable or not at a time of development. We all know that a piece of code evolves with time. So, this is important to measure the complexity of a code in time. Here we will see how we can set up this tool to calculate complexity of a code. Also, we will see how we can integrate this in the CI pipelines so we can ensure code quality in time.

<!--more-->

### Project directory structure

```bash
.
├── README.md
├── __init__.py
├── easy_functions.py
├── difficult_functions.py
└── wily_report
    ├── css
    │   └── main.css
    ├── graph.htm
    └── index.html
```

### Create a virtual environment

Create a new environment to install all the libraries required.

```bash
conda create --name wily_envs
conda activate wily_envs
```

Activate the environment and install the required libraries.

### Install Wily

```bash
pip3 install wily pre-commit
```

### Initalise git in the folder

After installing the dependency, initalise the folder with git and make inital commit

```bash
git init
git add .
git commit -m "inital commit."
```

### Write Python function in a file

Create a Python file say, `easy_functions.py`. Define the following function like,

```python
def easy_function(a, b):
    if a > 0 and b > 0:
        c = a + b
    return c
```

Now, create anoter file called `difficult_functions.py`.

```python
def difficult_function(a, b):
    if a > 0:
        if b > 0:
            if isinstance(a,int):
                if isinstance(b,int):
                    c = a + b
                    return c
                else:
                    pass
            else:
                pass
        else:
            pass
    else:
        pass
```

Add files in the git staging area.

```bash
git add .
git commit -m "adding new files and functions in the staging area."
```

### Check and create Wily report

```bash
# For help
wily --help
# To build report for all the functions in the current directory
wily build .
# Generate function-wise report
wily report easy_functions.py
wily report difficult_functions.py
# Index all the function
wily index
# Rank all the functions
wily rank .
# Check difference between last and current commit
wily diff . -r HEAD^1
# Check all the present matricies
wily list-metrics
# Generate a HTML report for the exiting functions
wily report --format HTML .
```

The generated graph for the functions will look like [this](https://pandalearnstocode.in/wily_graph_report/wily-report.html).

### Generate Wily graph

```bash
wily graph . raw.loc
wily graph easy_functions.py loc
```

The graph for the function's complexicity will be plotted in the generated report and this will look like [this](https://pandalearnstocode.in/wily_report/index.html).
This can be done for a function or a directory.

### Integrate with per-commit web hook

There is a pre-commit hook and azure pipelines defined for this tool which can be implimented for CI.

#### pre-commit plugin

Create a file called, `.pre-commit-config.yaml`.

The content of the file can be as follows,

```bash
repos:
-   repo: local
    hooks:
    -   id: wily
        name: wily
        entry: wily diff
        verbose: true
        language: python
        additional_dependencies: [wily]

```

Similarly an Azure pipeline can be defined as follows,

```bash
- script: |
  pip install wily
  wily build src/
  displayName: Install Wily and compile cache
- script: "wily diff src/ -r HEAD^1"
  displayName: Compare previous commit
```

After saving check that the `pre-commit` library is installed or not using,

```bash
pre-commit --version
```

Install the pre-commit hook for the respective directory,

```bash
pre-commit install
pre-commit run --all-files
git add .
git commit -m "commit after installing pre-commit hook"
conda deactivate
```

After this point the repo should be enabled with Wily.

### Take a informaed decesion

To simplify our life Wily provides us a MI which stands for code maintainablity index. If the index is in between 50 to 75, it needs improvement but anything less than 50 is not acceptable. Most of the companies like Microsoft and Java maintain there MI between 75 to 100.


## References

* [Pycon 2019 : Wily Python: Writing simpler and more maintainable Python](https://www.youtube.com/watch?v=dqdsNoApJ80)
* [Anthony Shaw - Wily Python: Writing simpler and more maintainable Python](https://speakerdeck.com/pycon2019/anthony-shaw-wily-python-writing-simpler-and-more-maintainable-python)
* [Refactoring in Python](https://realpython.com/python-refactoring/)
