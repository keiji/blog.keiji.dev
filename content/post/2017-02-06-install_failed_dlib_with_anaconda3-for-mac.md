---
title: Anaconda3 for macでdlibがインストールできない
author: Keiji Ariyama
type: post
date: 2017-02-06T12:46:19+00:00
url: /2017/02/install_failed_dlib_with_anaconda3-for-mac.html
categories:
  - メモ
  - 雑記

---
  * macOS Sierra &#8211; 10.12.3
  * Anaconda3 for OS X Python 3.6 version &#8211; 4.3.0
  * Boost &#8211; 1.63.0
  * dlib &#8211; 19.1.0

　MacBook Proを新規セットアップ中、`pip`で`dlib`をインストールするときにエラーが発生した。

<!--more-->

　最初は、brewでインストールした`Boost（1.63.0）`のバージョンが違っているのかと思ったのだけれど、要するに、Python 3.6に関連したdylib(macOSのダイナミックライブラリ)が見つからないと言うことらしい。

    $ pip install dlib
    
    ...
    
    make[2]: *** No rule to make target `/Users/keiji_ariyama/anaconda3/lib/libpython3.6.dylib', needed by `dlib.so'.  Stop.
    make[1]: *** [CMakeFiles/dlib_.dir/all] Error 2
    make: *** [all] Error 2
    error: cmake build failed!
    
    ----------------------------------------
    Command "/Users/keiji_ariyama/anaconda3/bin/python -u -c "import setuptools, tokenize;__file__='/private/var/folders/6n/.../T/pip-build-h7f8bau7/dlib/setup.py';f=getattr(tokenize, 'open', open)(__file__);code=f.read().replace('\r\n', '\n');f.close();exec(compile(code, __file__, 'exec'))" install --record /var/folders/6n/.../T/pip-dnhybqp0-record/install-record.txt --single-version-externally-managed --compile" failed with error code 1 in /private/var/folders/6n/.../T/pip-build-h7f8bau7/dlib/
    

　ファイルシステムを検索してみると、確かにAnaconda3に`libpython3.6.dylib`というファイルは含まれていない。あったのは`libpython3.6m.dylib`。

    $ ln -s libpython3.6m.dylib libpython3.6.dylib
    

　と、シンボリックリンクを張ることで解決した。

    $ pip install dlib
    Collecting dlib
      Using cached dlib-19.1.0.tar.gz
    Building wheels for collected packages: dlib
      Running setup.py bdist_wheel for dlib ... done
      Stored in directory: /Users/keiji_ariyama/Library/Caches/pip/wheels/b8/dc/75/...
    Successfully built dlib
    Installing collected packages: dlib
    Successfully installed dlib-19.1.0