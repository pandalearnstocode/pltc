---
title: Docker for Data Science
author: Aritra Biswas
date: '2019-08-25'
slug: docker-for-data-science
categories:
  - Data Science
tags:
  - docker
cover: /img/python_img/docker.png
---

One of the key problem which I have encountered while working in data science domains for couple of years is reproducibility. This is true for any form of research but it has special importance in data science domain especially if you are working with a code which will be consumed by others. If you are dealing with a code which is not reproducible then you can fix it but if your code is used by others there is no one to fix the problem which is not an error in true sense. There are a couple of ways in which you can handle this problem in almost all the programming languages but certainly code is not going to with the infrustructure in which it is developed (like hardware, driver, OS and other system libraries) . To solve this problem I preffer using docker and I very much like it. Docker has its pros and cons. According to many people, there is some good alternative available which can replace docker (like podman) but in my personal opinion docker is still the best option for a person which is not very much into DevOps but wants to get this problem fixed. Here are the other reasons why I feel docker is a better choice.

## Why docker is important:

* Easy to use and learn. Open source community support and literature. 
* Great documentation and community support. 
* Almost all the leading key cloud service provider has support for this at present.
* A lot of prebuild images available which can be plugged in as different component other than investing time in those.
* Easy to create, manage multiple replicas for an instance which can be leveraged for large scale computation.
* Easy to share and colaborate without leaving the existing platform.
* Many organizations which uses legacy server or cloud services has certain restriction regarding port opening and sudo access. It is possible if you have docker installed in the server to use these functionalities within the docker container.
* You can easily share your work and manage dependency.

## Important things to remember while using docker

Here are some of the useful things which I have figured out from my experiences:

* Do not build an image. Find an image which can suit your purpose. There are multiple official images available online.
* Be true that what you want to achieve by using docker. For example, making an image which will suffice the purpose of reproducibility will be very much different than building a lightweight image which can be replicated many times to accommodate multiple users or to perform a task which is heavy on a single CPU.
* There are other tools such as kubernetes and docker-compose. Before starting anything be sure about the use case.  
* There are multiple ways to achieve a task in this domain. The approch which works for you is the correct one unless there is some other security vulnerability. 
* Docker images can consume huge memory if not cleaned and deleted unused images on a regular basis. This is important to know which images are present in your system and not used and to delete them manually.
* Docker is agnostic of the system hardware. So the performance will vary due to change in hardware.

## Best practises to build a docker image

Here are some of the best practice which can help you to build a useful docker image:

> There is a version of Alpine Linux which will consume only ~5 MB disk location where ubuntu will take ~190 MB.

* Use minimalistic docker image as a base which can suffice the purpose. By listing to the above statement it is quite natural that one may feel that this is the best choice one can have for everything they want to build. But in reality that is not the case, so do not go for Alpine Linux all the time. Be sure that the project in which you are working has a use case of using a smaller image. If it does not increase efficiency, do not choose something which may lead to frustration and many difficulties. For example, the raw Alpine Linux does not come with basic dependencies for data science such as GCC , bash, pip and many more. So every feature you need has to be installed manually. Many R, Python, Julia libraries will assume those dependencies are present in the OS and if they are not present it will throw an error while install these libraries. Installing these manually can always break the system if someone does not has the expertise to understand which can go with what.
* Use multi-stage docker containers if you are concerned about security or image size. `COPY`, `ADD` these sort of commands in Docker needs certain dependencies which can be ignored if someone uses multi-stage docker container. 
* Clear all the system dependencies after installing a package or library. For example, if you are using pip for python use a build directory and delete the build directory after the successful installation of the package or if you are using conda to manage packages for python use `conda clean --all` to make sure the dependencies which have been downloaded compile the library is no longer stored.
* PIN every possible core dependency including image name, version, tags, dependency management tools as well.  
* Be sure about the source from which the library is being downloaded. For example `numpy==1.16.XX` from origin coda-forge can be very different from if you download it from the `conda` channel. So, not only the package version is sure that the source from which this package is being downloaded also records and used while installing.
* DO NOT `COPY` or `ADD` dependency file i.e. `requirements.txt` or `environment.yml` with the other files. Copy it before installing the packages, install the packages and then do if there is anything else has to be done. 
* Always use a strategy while updating or installing a package. Make sure that the newly installed package does not change the installed version of existing packages if they have a dependency on them.
* Never hard code credential, keys or password in Dockerfile. Pass the as build argument or environment variable. 
* Once an image is built all the code and credentials are stored in that image. So, in case if you are planning to host those image with embedded code my be docker hub is not the ideal place. Although for an image without and source code docker hub is the easiest place to store images. Other than docker hub it is possible to create a docker registry in any other server. Most of the cloud provider now days have their docker container registry service. Azure container registry is one of the easiest solutions with minimal setup required. 
* DO NOT update anything on the go. That defeats the purpose of the whole thing and this can have a snowball effect which may end up ruining all the effort.

So, these are some use cases and experiences because of which docker is one of the defacto tool in my day to day data science work. There will some detailed posts related to docker which will explain these concepts in a detailed manner.

### __Note:__

* Docker image and docker containers are not the same thing. Image is something where you build a virtual hard disk which is spinned up to provide service. When all the things need to provid the services are bundeled togather is a docker image. Now when we start that image and start providing the required services the running instance of the image is referred to a container. One image can be spinned as multiple container where the base image will be same. 
* Although it is mentioned previously. This is important to mention again that NEVER pass a RSA tocken, user id, password, key phrase as hardcoded variable in Dockerfile. Use build argument or enviourment variable to pass sensitive information. If embeding RSA token is absolutely important to fetch data from github or bitbucket make sure that this has been implimented through multi-stage docker container.
* Try avoiding `squash`. If it has to be done then do it when you are quite sure that the docker image does not need anything more. Using squash will delete all the intermediate builds and later will not be able to fetch data from cache.
* If you are familar with Visual Studio Code then it has some really useful packages which makes life easier. At present it is possible to attach a docker container with Visual Studio Code. If you are using Apline Linux then you have to use Visual Studio Code Insider. For other varients the you can connect to a docker container and code using it. It will make sure the development and production enviourment is exactly the same. 

### __Reference:__

* [Drastically reduce the size of your docker images with MULTISTAGE builds ](https://www.youtube.com/watch?v=KLOdisHW8rQ)
* [Docker offical documentation](https://docs.docker.com/)
* [Basics of docker compose](https://www.youtube.com/watch?v=4EqysCR3mjo)