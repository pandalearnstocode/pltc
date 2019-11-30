---
title: Record and share terminal session using Terminalizer
author: Aritra Biswas
date: '2019-11-30'
slug: record-and-share-terminal-session-using-terminalizer
categories:
  - Data Science
tags:
  - Documentation
  - Terminal
cover: "/img/python_img/terminal.png"
---

There are great open-source tools are available to record screen. I personally like [loom](https://www.loom.com/) and google hangout. All of these are good. But I recently came across Terminalizer to record the terminal session. It is easy to set up and use. Here in this session, I will quickly demo how I use this tool for document creation and sharing.


<!--more-->


## Dependency

Need to install [Node.js](https://nodejs.org/en/download/) before installing this tool.

## Official documentation

Here is the [official github repo](https://github.com/faressoft/terminalizer) of the project. [This](https://terminalizer.com/) is the site with a lot of example and installation instruction.

### Record gifs

```shell
terminalizer record demo
```
### Play recorded gif in terminal

```shell
terminalizer play demo
```

### Play recorded gif

```shell
terminalizer render demo
```

### The generated gif will something like this,

![](/img/python_img/helloworld_min.gif)

### gif compression

If one is planning to use these gifs in website it is a good idea to compress the gifs before uploading. [This](https://gifcompressor.com/) site is useful for compression.

### HTTPie—aitch-tee-tee-pie

The official website for this project can be found [here](https://httpie.org/). If you are in Mac you can use the following command from terminal to install this tool:

```shell
brew install httpie
```

### Test HTTPie and Terminalizer with a FastAPI Hello World App

Here is the app which we are going to use for the demo,

```python
# Import dependencies
from fastapi import FastAPI
from pydantic import BaseModel
# Define App
app = FastAPI()
# Define request body class
class Name(BaseModel):
    name: str
# Define Endpoint
@app.post("/name/")
async def name_return(name:Name):
    return name
```

To start the app use the following command from commandline,

```shell
pip3 install virtualenv
python3 -m virtualenv
source ./bin/activate
pip3 install uvicorn pydantic fastapi[all]
uvicorn main:app --reload
```
If the app is up, you check the the Swagger UI [here](http://localhost:8000/docs). This should look like this,


![](/img/python_img/swaggerui.jpg)

To make a `POST` call to the API one can use Httpie from terminal by using the following command,

```shell
http POST localhost:8000/name/ name='Aritra Biswas'
```

The output of this will look like,

![](/img/python_img/httpie_mini.gif)
