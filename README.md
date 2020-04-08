---
title: "How read all files from google colab"
date: 2020-04-07 11:33:00 +0800
categories: [Blogging, Python]
tags: [python, ios, pandas, google_colab]
---

# What is Colaboratory?

Colaboratory, or "Colab" for short, allows you to write and execute Python in your browser, with

 + Zero configuration required
 + Free access to GPUs
 + Easy sharing
 
Whether you're a student, a data scientist or an AI researcher, Colab can make your work easier. Watch Introduction to Colab to learn more, or just get started below!

Beside all those benefits, we can access data in Google driver from google colab. 


```python
from google.colab import drive
drive.mount('/content/gdrive', force_remount=True)
root_dir = "/content/gdrive/My Drive/fiverr"
base_dir = root_dir + 'byamba-v3/'
```
os.listdir() will get you everything that's in a directory - files and directories.

If you want just files, you could filter this down using os.path:

```python
from os import listdir
from os.path import isfile, join
mypath='/content/gdrive/My Drive/fiverr/frank/COVID-19/csse_covid_19_data/csse_covid_19_daily_reports'
onlyfiles = [f for f in listdir(mypath) if isfile(join(mypath, f))]
```
or you could use os.walk() which will yield two lists for each directory it visits - splitting into files and dirs for you. If you only want the top directory you can just break the first time it yields

```python
from os import walk

f = []
for (dirpath, dirnames, filenames) in walk(mypath):
    f.extend(filenames)
    break
```


```python 
import pandas as pd
import numpy as np
```


```python
def concat_files():
  dfs=[]
  df1=pd.read_csv(onlyfiles[1])
  for i in range(2,76):
    df=pd.read_csv(onlyfiles[i])
    if df.shape[1]==6:
      dfs.append(df)
  large=df1.append(dfs)
  return large
```