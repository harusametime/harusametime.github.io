---
title: 'WindowsへのPySPQRのインストール'
date: 2017-10-06
permalink: /posts/2017/10/install_pyspqr/
tags:
  - Optimization
  - Python
---

## PySPQRをインストールしたい

- ScipyにはSparse matrix向けのQR分解がありません.
- Dense matrix向けはありますが，todense()するとメモリで死んでしまいます．
- SuiteSparseを使うとできるけどpythonで使えない.
- Python wrapperの[PySPQR](https://github.com/yig/PySPQR)を使う．

## 環境
- Windows 10
- Microsoft Visual Studio 14.0 (SuiteSparseのコンパイルにいる）
- Python 3.5 (2.7でも成功しました)

## SuiteSparseをインストール
公式にはないけど，windows用のインストール用CMakeがある．
- suitesparse-metis-for-windowsを使う．以下のURLの手順に従う．
- https://github.com/jlblancoc/suitesparse-metis-for-windows
- (4)CMakeで必ず自分のVisual StudioのVersionを選ぶこと．(4)の他のステップはしなくてもよかった．
- (5)Visual Studioでコンパイルすると，build/lib/Releaseフォルダができて，そこにlibファイルができる（あとで使う）

## PySPQRをインストール
- とりあえずdirect installを試す．
  - 色々とやり方[https://github.com/yig/PySPQR](https://github.com/yig/PySPQR)が書いてありますが，Directlyを選びます．
  - つまり，PySPQRのプロジェクトをダウンロードし，sparseqrのフォルダをPythonのプロジェクトにぶち込みます．
  - pythonのファイルからimport sparseqrしてpythonを実行してみます．
- ヘッダファイルが無いよというエラーが出ます．
  ~~~
  C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\BIN\x86_amd64\cl.exe
  /c /nologo /Ox /W3 /GL /DNDEBUG /MD -I/usr/include/suitesparse
  -IC:\Users\(なんとかかんとか)\include (なんとかかんとか)
  "SuiteSparseQR_C.h" (そんなファイル無い的な)
  ~~~
  
  - suitesparse-metis-for-windowsのフォルダのどこかにヘッダファイルがあります．
  - 探してエラーメッセージ中のC:\Users\(なんとかかんとか)\includeとかに入れてあげます．
  - 必要なヘッダファイルは1つではありません．pythonを実行して，エラーがなくなるまで入れ続けます．
- ヘッダファイルを全部入れるとlibファイルが無いよと言ってきます．

  ~~~
  C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\BIN\x86_amd64\link.exe /nologo
  /INCREMENTAL:NO /LTCG /DLL /MANIFEST:EMBED,ID=2   /MANIFESTUAC:NO /LIBPATH:C:\Users\（なんとかかんとか）\libs
  /LIBPATH:C:(なんとかかんとか）LINK : fatal error LNK1181: 入力ファイル 'spqr.lib' を開けません。
  ~~~
  
  - このlibファイルは前述のbuild/lib/Releaseフォルダ以下のlibファイルです．ファイル名の冒頭に余計なlibがついているので消してコピーします．

- しかしまだうまく行きません．

  ~~~
  ライブラリ .\Release\sparseqr\_sparseqr.cp35-win_amd64.lib とオブジェクト
    .\Release\sparseqr\_sparseqr.cp35-win_amd64.exp を作成中
  _sparseqr.obj : error LNK2001: 外部シンボル "cholmod_l_check_triplet" は未解決です。...
  ~~~
  
  - PySPQRのsparsesqrフォルダにあるparseqr_gen.pyを編集します．
  - 19行目のlibrariesでspqrだけ指定されていますが，ぶちこんだlibを全部指定します．
  
  ~~~
  libraries=['amd','btf','camd','ccolamd','cholmod','colamd','cxsparse','klu','lapack','ldl','lumfpack','metis',
  'suitesparseconfig','spqr','libblas'])
  ~~~

- DLLが無いとか言ってくるんですが．．．

  - suitesparse-metis-for-windowsのlib64\lapack_blas_windowsの中のdllを全部sparseqrのフォルダにぶち込む．
  
- これでpythonを実行すればうまく行きました．お疲れさまでした．
