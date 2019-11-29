---
title: 'Tutorial: Quick EDA in Python using Pandas Profiling'
author: Aritra Biswas
date: '2019-11-29'
slug: tutorial-quick-eda-in-python-using-pandas-profiling
categories:
  - Data Science
tags:
  - Python
  - EDA
  - pandas
cover: "/img/python_img/pandas-logo.png"
---

EDA is one of the most important part of any data science work. Recently I have came accross a library called pandas profiling which makes it easier and creates a beautiful shareable report. So, here I am sharing some resouces related to  the project which I feel can be useful.

<!--more-->

## Creating virtual envs and installing dependencies

### Dependency

Anaconda and Python 3 has to be installed. If not check this [link](https://docs.anaconda.com/anaconda/install/) to install Anaconda first.

### Create and activate envs
```bash
conda create --name pandasprofiling
conda activate pandasprofiling
```
### Install library dependencies

```bash
conda install -c anaconda pandas jupyter notebook
conda config --add channels conda-forge
conda install -c conda-forge pandas-profiling
```

```bash
jupyter notebook
```

### Using pandas profiling

#### Import dependency and data

```python
import pandas as pd
import pandas_profiling
url = "https://gist.githubusercontent.com/seankross/a412dfbd88b3db70b74b/raw/5f23f993cd87c283ce766e7ac6b329ee7cc2e1d1/mtcars.csv"
mtcars = pd.read_csv(url)
```

#### Data type and numerical check

```python
# glimps of data
mtcars.head()
# descriptive stats by variable
mtcars.describe()
# data type and sub types by variable
pd.DataFrame(mtcars.dtypes,columns=['datatypes'])
```

#### Generate and save report

```python
# generated eda for the dataset
mtcars.profile_report(style={'full_width':True})
# save report in an object
mtcars_profile = mtcars.profile_report()
# filter variables out from the object on the basis of a cutoff
mtcars_profile.get_rejected_variables(threshold=0.9)
# create a report with a title
profile = mtcars.profile_report(title='EDA of mtcars dataset using Pandas Profiling')
# save report with the title
profile.to_file(output_file="mtcars_eda.html")
# create a minified report
profile_minified = mtcars.profile_report(title='EDA of mtcars dataset using Pandas Profiling',minify_html=True)
# save the minified report
profile_minified.to_file(output_file="mtcars_eda_minified.html")
```

Here is the copy of the generated report [mtcars EDA](/html_report/mtcars_eda.html) and [mtcars EDA minified](/html_report/mtcars_eda_minified.html).

### Deactivate conda

```bash
conda deactivate
```
### Interprate report

#### Check if the is a huge amount of correlation amount the variables in the following section.

![](/img/python_img/pandas_profiling_1.jpg)

#### Check descriptive statistics for numeric variables in the following section. This will give an idea about the distribution of the variable.

![](/img/python_img/pandas_profiling_2.jpg)

#### From the histogram of the variable one can have an idea about the population distribution of the variable.

![](/img/python_img/pandas_profiling_3.jpg)

#### Correlation heatmap of this section can be used to understand multi-colinearity among the variables at the same time this can be used for variable selection.

![](/img/python_img/pandas_profiling_4.jpg)

#### This heatmap can be used to understand the measure of association between categorical variables.

![](/img/python_img/pandas_profiling_5.jpg)

#### The bar chart shown bellow can be used to understand the missing values for each variable.

![](/img/python_img/pandas_profiling_6.jpg)


### Reference

* [Speed Up Your Exploratory Data Analysis With Pandas-Profiling](https://towardsdatascience.com/speed-up-your-exploratory-data-analysis-with-pandas-profiling-88b33dc53625)
* [Pandas Profiling Official Doc](https://pandas-profiling.github.io/pandas-profiling/docs/)
* [Pandas Profiling To Boost Exploratory Data Analysis](https://blog.usejournal.com/pandas-profiling-to-boost-exploratory-data-analysis-8e718238bcd1)
* [An Introduction to Pandas Profiling](https://medium.com/analytics-vidhya/pandas-profiling-5ecd0b977ecd)
* [Intro to pandas_profiling - Simple Fast EDA](https://www.kaggle.com/nulldata/intro-to-pandas-profiling-simple-fast-eda)