---
title: Optimized docker images for Data Science With FastAPI
author: Aritra Biswas
date: '2019-07-21'
slug: optimized-docker-images-for-data-science-with-fastapi
categories:
  - Data Science
tags:
  - docker
  - numba
  - dask
  - numpy
  - pandas
  - FastAPI
cover: "/img/python_img/docker.png"
---

Lets build a container based application which is optimized in terms of resources and integrate FastAPI with it. It will help to interact the app to connect to the internet and at the same time we will try to get the bast of data science stack in python. Here is the basic layout which we will try to achieve,


<!--more-->

1. Alpine linux based docker container.
2. Miniconda
3. MKL, LLVM, SVML, ICC RT or Open BLAS
4. Pandas, Numpy, Numba and Dask
5. FastAPI


```bash
# Starting from a minimalistic alpine and miniconda base image
FROM frolvlad/alpine-miniconda3:python3.6

ENV PYTHONDONTWRITEBYTECODE=true

RUN conda update -n base -c defaults conda

RUN conda install --yes --freeze-installed \
    nomkl \
    numpy==1.16.3 \
    pandas==0.24.2 \
    numba==0.44.1 \
    dask==2.1.0 \
    && conda clean -afy \
    && conda clean --all \
    && find /opt/conda/ -follow -type f -name '*.a' -delete \
    && find /opt/conda/ -follow -type f -name '*.pyc' -delete \
    && find /opt/conda/ -follow -type f -name '*.js.map' -delete \
    && find /opt/conda/lib/python*/site-packages/bokeh/server/static \
    -follow -type f -name '*.js' ! -name '*.min.js' -delete

RUN apk add --no-cache --virtual .build-deps gcc libc-dev make \
    && pip --no-cache-dir install uvicorn gunicorn fastapi pydantic \
    && apk del .build-deps gcc libc-dev make

RUN rm -rf /var/cache/apk/* && \
    rm -rf /tmp/* && \
    rm -rf ~/.cache/pip

COPY ./start.sh /start.sh

RUN chmod +x /start.sh

COPY ./gunicorn_conf.py /gunicorn_conf.py

COPY ./start-reload.sh /start-reload.sh

RUN chmod +x /start-reload.sh

COPY ./app /app

WORKDIR /app/

ENV PYTHONPATH=/app

EXPOSE 80

CMD ["/start.sh"]
```

```bash

docker build -t fa_mc_blas_dss .

docker run -d -p 80:80 fa_mc_blas_dss

docker run -it fa_mc_blas_dss sh

docker stop $(docker ps -aq)

```

```bash

FROM frolvlad/alpine-miniconda3:python3.6

ENV PYTHONDONTWRITEBYTECODE=true

RUN apk add --no-cache --virtual .build-deps gcc libc-dev make \
    && pip --no-cache-dir install uvicorn gunicorn fastapi pydantic \
    && apk del .build-deps gcc libc-dev make

RUN conda config --add channels intel \
    && conda install -y --freeze-installed  -c intel numpy==1.16.3 \
    && conda install -y --freeze-installed -c intel numba==0.44.1 \
    && conda install -y --freeze-installed -c intel pandas==0.24.2 \
    && conda install -y --freeze-installed -c intel dask==2.1.0 \
    && conda clean --all \
    && conda clean -afy \
    && find /opt/conda/ -follow -type f -name '*.a' -delete \
    && find /opt/conda/ -follow -type f -name '*.pyc' -delete \
    && find /opt/conda/ -follow -type f -name '*.js.map' -delete \
    && find /opt/conda/lib/python*/site-packages/bokeh/server/static \
    -follow -type f -name '*.js' ! -name '*.min.js' -delete

RUN rm -rf /var/cache/apk/* && \
    rm -rf /tmp/* && \
    rm -rf ~/.cache/pip

COPY ./start.sh /start.sh

RUN chmod +x /start.sh

COPY ./gunicorn_conf.py /gunicorn_conf.py

COPY ./start-reload.sh /start-reload.sh

RUN chmod +x /start-reload.sh

COPY ./app /app

WORKDIR /app/

ENV PYTHONPATH=/app

EXPOSE 80

CMD ["/start.sh"]
```

```bash

docker build -t fa_mc_mkl_svml_dss .

docker run -d -p 80:80 fa_mc_mkl_svml_dss

docker run -it fa_mc_mkl_svml_dss sh

docker stop $(docker ps -aq)

```


