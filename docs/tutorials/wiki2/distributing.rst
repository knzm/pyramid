.. Distributing Your Application

=============================
アプリケーションを配布する
=============================

.. Once your application works properly, you can create a "tarball" from
.. it by using the ``setup.py sdist`` command.  The following commands
.. assume your current working directory is the ``tutorial`` package
.. we've created and that the parent directory of the ``tutorial``
.. package is a virtualenv representing a :app:`Pyramid` environment.

あなたのアプリケーションが正常に動作したら、 ``setup.py sdist`` コマンドを使ってそこから "tarball" を作成できます。
次のコマンドはあなたの現在の作業ディレクトリが私たちによって作成された ``tutorial`` パッケージで、
``tutorial`` パッケージの親ディレクトリは :app:`Pyramid` 環境を表すvirtualenvであると想定しています。


.. On UNIX:

UNIXの場合:


.. code-block:: text

   $ ../bin/python setup.py sdist


.. On Windows:

Windowsの場合:


.. code-block:: text

   c:\pyramidtut> ..\Scripts\python setup.py sdist


.. The output of such a command will be something like:

コマンドの出力は以下のようになります:


.. code-block:: text

   running sdist
   # ... more output ...
   creating dist
   tar -cf dist/tutorial-0.0.tar tutorial-0.0
   gzip -f9 dist/tutorial-0.0.tar
   removing 'tutorial-0.0' (and everything under it)


.. Note that this command creates a tarball in the "dist" subdirectory
.. named ``tutorial-0.0.tar.gz``.  You can send this file to your friends
.. to show them your cool new application.  They should be able to
.. install it by pointing the ``easy_install`` command directly at it.
.. Or you can upload it to `PyPI <http://pypi.python.org>`_ and share it
.. with the rest of the world, where it can be downloaded via
.. ``easy_install`` remotely like any other package people download from
.. PyPI.

なお、このコマンドは ``tutorial-0.0.tar.gz`` という名前で "dist" サブディレクトリにtarballを作成します。
あなたはこれらのクールな新しいアプリケーションを見せるために、友達にこのファイルを送ることができます。
彼らはこれに直接 ``easy_install`` コマンドをポイントすることによってインストールできるようになるはずです。
または、これを `PyPI <http://pypi.python.org>`_ にアップロードし、国外と共有することができます。
ここでPyPIから他人がリモートで他のパッケージをダウンロードしているみたいに ``easy_install`` を使ってこれをダウンロードできます。
