---
title: 'Why is joblib in docker so slow?'
date: 2017-12-12
permalink: /posts/2017/12/joblib_docker/
tags:
  - Python
---

Joblib in a docker container is slow when handling big arrays. This shows a workaround for the problem.

## Problem
When passing big arrays to joblib.Parallel in docker container, parallel processing does not start immediately. 

## Why?
Joblib uses the folder specified by "JOBLIB_TEMP_FOLDER" for memmap of the arrays. Without specifying, JOBLIB_TEMP_FOLDER is set to `/dev/shm`, which usually has small size and is not enough for the big arrays.

https://pythonhosted.org/joblib/generated/joblib.Parallel.html

## Solution
-  Set JOBLIB_TEMP_FOLDER
-  Or specify the folder when running the parallel processing

```python
from sklearn.externals.joblib import Parallel, delayed
r = Parallel(n_jobs=-1, temp_folder=".")(delayed(hogehoge)(bigarray[i] for i in List_i))
```

(In Japanese)

docker上のjoblibのParallelで大きいサイズのデータを渡すと，並列処理がなかなか全く始まらないことがある．
joblibは，memmap用の領域として，JOBLIB_TEMP_FOLDERで指定したフォルダを利用するが，指定がない場合/dev/shmを利用する．

しかしdockerではデフォルトの/dev/shmの領域が非常に小さく，上記の問題を引き起こす．
解消するためには，JOBLIB_TEMP_FOLDERを指定するか，Parallelの引数temp_folderを指定する．
https://pythonhosted.org/joblib/generated/joblib.Parallel.html
