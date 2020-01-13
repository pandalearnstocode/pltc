---
title: Automated documentation using pdoc3
author: Aritra Biswas
date: '2020-01-13'
slug: automated-docstring-using-pdoc3
categories:
  - Data Science
tags:
  - Documentation
  - Code Quality
  - Best practices
  - Python
cover: "/img/python_img/pdoc_logo.png"
draft: flase
---

Documentation of a project is one of the essential aspects in terms of maintainability. Here, in this article, we will cover pdoc3, a python library which can create documentation quickly without any bottleneck. This library is quite useful for creating documentation for backend or middleware in my mind. I find it challenging to use with math symbols in this, but document generation and hosting quite easy. Pdoc is quite easy to use in comparison with Sphinx, which is quite good with LaTeX symbol and other fancy things.

<!--more-->

Just one point if you are planning to document a project it has to be good because,

![https://pandalearnstocode.in/slide/pdoc_funny.jpg](https://pandalearnstocode.in/slide/pdoc_funny.png)


## List of required dependencies for this project

Here is the list of libraries which will be used for this project. Create a `requirements.txt` file in the root directory of the project and paste the following content.

```python
aiofiles==0.4.0
pdoc3==0.7.2
fastapi==0.46.0
```
Save the above content in the `requirements.txt` file, and we are good to go.

## Installing dependency in the system

Here I am assuming that the anaconda distribution of python 3 is installed in the system. If it is not, then you can download it from [here]([https://www.anaconda.com/distribution/](https://www.anaconda.com/distribution/)). 

```shell
conda create --name documentation --file requirements.txt
conda activate documentation
```
Now, create a virtual environment using Conda package manager. Here we are naming this environment as `documentation`, and we are specifying that the dependencies which will be installed in the virtual environment will come from the `requirements.txt` file in the root directory. After the environment is successfully created, activate the environment.

## Install autodoc extension in Visual Studio Code

If you have visual studio code installed the install [this]([https://marketplace.visualstudio.com/items?itemName=njpwerner.autodocstring](https://marketplace.visualstudio.com/items?itemName=njpwerner.autodocstring))  plugin from the market place. Go to setting and select default docstring generation style to be numpy. But it is not exactly numpy. The third bracket has to be removed from the args and returns definition like the `add_two_numbers.py` file, and it will work fine.

## Create the required project structure

After executing the step mentioned above create the following project structure,

```shell
touch README.md
mkdir documentation && cd documentation && touch add_two_numbers.py
cd ..
mkdir html && cd html && touch doc_app.py && cd ..
```

These steps will create two folders named `documentation` and `HTML` in the root folder. Create `add_two_numbers.py` in the  `documentation` folder and `doc_app.py` in the HTML folder. Here are the contents for the folders mentioned,

### Content of `add_two_numbers.py`file

```python
def  addition(first_number: int, second_number: float) -> float:
	"""This function will perform addition of two numbers.
	Parameters
	----------
	first_number : int
		the 1st arg has to be an int
	second_number : float
		the 2nd arg has to be an float
	Returns
	-------
	float
		the result of sum of an int and a float will be a float
	"""
	result: float  = first_number + second_number
	return result
```

### Content of `doc_app.py` file

```python
from fastapi import FastAPI
from starlette.staticfiles import StaticFiles
app = FastAPI()
app.mount("/documentation", StaticFiles(directory="documentation"))
```

### Convert docstring to HTML files using pdoc3

To generate HTML files from the docstring use the following command.

```shell
# In development
pdoc --html documentation
# In staging or production
pdoc --html --config show_source_code=False documentation
```
### Final structure of the project

If everything in the previous steps mentioned goes well, then the project structure will be as follows,

![https://pandalearnstocode.in/slide/doc_dir_str.jpg](https://pandalearnstocode.in/slide/doc_dir_str.jpg)  

## Serve HTML site using Fast API

Check the HTML files generated after this process manually, or else it is possible to serve these HTML files using a webserver. It will help to see the changes done immediately on the fly. It will help to host server when the project is hosted in the server.

So, to start the webserver to host generated HTML file use the following commands,

### Start and access HTML file using doc_app server

```shell
cd html && uvicorn doc_app:app --reload
open -a "Google Chrome" http://127.0.0.1:8000/documentation/index.html
```
This will open the HTML pages. Here `doc_app` is the file name, and `app` is the module name. In the URL `documentation` is the name of the module. It will change if the file name, app name or the module name changes.

## Extract dependency from the existing environment

It can be essential to install various packages during this setup. If you have installed any other dependency in mentioned in the document, dump it in `requirements.txt` file so that it can be reproduced.

```shell
pip3 freeze > requirements.txt
```

## Deactivate environment

After everything is done to keep things in the same state, do not forget to deactivate the environments.

```shell
conda deactivate
```

## Further scope of improvement

Create a `bash` script or a pre-commit hook. So, when a commit is made, this document can be generated on the fly. These files will be later hosted to a server so that the users can access this HTML site without any manual intervention whenever a push it made.