---
title: Record and share terminal session using Terminalizer
author: Aritra Biswas
date: '2019-11-30'
slug: record-and-share-terminal-session-using-terminalizer
categories:
  - Data Science
tags:
  - Documentation
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
