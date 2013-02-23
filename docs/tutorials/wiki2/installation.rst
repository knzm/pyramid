.. Installation

============
インストール
============

.. Preparation

準備
===========

.. Follow the steps in :ref:`installing_chapter`, but name the virtualenv
.. directory ``pyramidtut``.

:ref:`installing_chapter` の手順に従ってください。ただし virtualenv
ディレクトリの名前は ``pyramidtut`` にしてください。


.. Preparation, UNIX

準備 (UNIX)
-----------------

.. #. Install SQLite3 and its development packages if you don't already
..    have them installed.  Usually this is via your system's package
..    manager.  On a Debian system, this would be:

1. まだインストールしていなければ SQLite3 とその開発用パッケージの
   インストール。通常これはシステムのパッケージマネージャーを介して行います。
   Debian Linuxシステムでは、以下のようになります:


   .. code-block:: text

      $ sudo apt-get install libsqlite3-dev


.. #. Switch to the ``pyramidtut`` directory:

2. ``pyramidtut`` ディレクトリに移動します:


   .. code-block:: text

      $ cd pyramidtut


.. Preparation, Windows

準備 (Windows)
--------------------

.. #. Switch to the ``pyramidtut`` directory:

1. ``pyramidtut`` ディレクトリに移動します:


   .. code-block:: text

      c:\> cd pyramidtut


.. Making a Project

.. _sql_making_a_project:

プロジェクトの作成
==================

.. Your next step is to create a project.  For this tutorial, we will use the
.. :term:`scaffold` named ``alchemy``, which generates an application
.. that uses :term:`SQLAlchemy` and :term:`URL dispatch`.  :app:`Pyramid`
.. supplies a variety of scaffolds to generate sample projects.

次のステップはプロジェクトを作成することです。このチュートリアルでは
:term:`scaffold` (骨組み) として ``alchemy`` を使用します。これは
:term:`SQLAlchemy` と :term:`URL dispatch` を使用するアプリケーションを
生成します。 :app:`Pyramid` はサンプルプロジェクトを作成するための様々な
scaffold を提供しています。


.. The below instructions assume your current working directory is the
.. "virtualenv" named "pyramidtut".

以下の説明では、現在の作業ディレクトリが "pyramidtut" という名前の
"virtualenv" であると仮定しています。


.. On UNIX:

UNIX の場合:


.. code-block:: text

   $ bin/pcreate -s alchemy tutorial


.. On Windows:

Windows の場合:


.. code-block:: text

   c:\pyramidtut> Scripts\pcreate -s alchemy tutorial


.. .. note:: If you are using Windows, the ``alchemy``
..    scaffold may not deal gracefully with installation into a
..    location that contains spaces in the path.  If you experience
..    startup problems, try putting both the virtualenv and the project
..    into directories that do not contain spaces in their paths.

.. note::

   Windows を使っている場合、 ``alchemy`` scaffold はパスにスペースが
   含まれている場所には簡単にインストールできないかもしれません。もし
   startup で問題に遭遇したら、 virtualenv とプロジェクトの両方を
   パスにスペースが含まれていないディレクトリに置いてみてください。


.. Installing the Project in "Development Mode"

開発モードでのプロジェクトのインストール
============================================

.. In order to do development on the project easily, you must "register"
.. the project as a development egg in your workspace using the
.. ``setup.py develop`` command.  In order to do so, cd to the `tutorial`
.. directory you created in :ref:`sql_making_a_project`, and run the
.. ``setup.py develop`` command using the virtualenv Python interpreter.

簡単にプロジェクトの開発をするために、 ``setup.py develop`` コマンドを
使ってプロジェクトを開発用 egg として作業用スペースに "登録" する必要が
あります。これをするために、 :ref:`sql_making_a_project` で作成した
`tutorial` ディレクトリに移動します。そして virtualenv の Python
インタプリタを使って ``setup.py develop`` コマンドを実行します。


.. On UNIX:

UNIX の場合:


.. code-block:: text

   $ cd tutorial
   $ ../bin/python setup.py develop


.. On Windows:

Windows の場合:


.. code-block:: text

   c:\pyramidtut> cd tutorial
   c:\pyramidtut\tutorial> ..\Scripts\python setup.py develop


.. Success executing this command will end with a line to the console something
.. like:

このコマンドの実行に成功すると、コンソールに次のような出力が行われるでしょう:


::

   Finished processing dependencies for tutorial==0.0


.. Running the Tests

.. _sql_running_tests:

テストの実行
=================

.. After you've installed the project in development mode, you may run
.. the tests for the project.

開発モードでインストールした後で、プロジェクトに対するテストを実行する
ことができます。


.. On UNIX:

UNIX の場合:


.. code-block:: text

   $ ../bin/python setup.py test -q


.. On Windows:

Windows の場合:


.. code-block:: text

   c:\pyramidtut\tutorial> ..\Scripts\python setup.py test -q


.. For a successful test run, you should see output that ends like this:

テストの実行に成功すると、次のような出力が見られるはずです:


::

  .
  ----------------------------------------------------------------------
  Ran 1 test in 0.094s
 
  OK


.. Exposing Test Coverage Information

テストカバレッジ情報の抽出
==================================

.. You can run the ``nosetests`` command to see test coverage
.. information.  This runs the tests in the same way that ``setup.py
.. test`` does but provides additional "coverage" information, exposing
.. which lines of your project are "covered" (or not covered) by the
.. tests.

``nosetests`` コマンドを実行してテストカバレッジを見ることができます。
これは ``setup.py test`` と同じ方法でテストを実行しますが、追加の
"カバレッジ" 情報を提供し、そのプロジェクトの行がテストで "カバーされて
いる" (もしくはカバーされていない) かを抽出します。


.. To get this functionality working, we'll need to install the ``nose`` and
.. ``coverage`` packages into our ``virtualenv``:

この機能を動かすには、 ``nose`` および ``coverage`` パッケージを
``virtualenv`` にインストールする必要があります。


.. On UNIX:

UNIX の場合:


.. code-block:: text

   $ ../bin/easy_install nose coverage


.. On Windows:

Windows の場合:


.. code-block:: text

   c:\pyramidtut\tutorial> ..\Scripts\easy_install nose coverage


.. Once ``nose`` and ``coverage`` are installed, we can actually run the
.. coverage tests.

``nose`` と ``coverage`` をインストールすると、実際にカバレッジテストが
実行できます。


.. On UNIX:

UNIX の場合:


.. code-block:: text

   $ ../bin/nosetests --cover-package=tutorial --cover-erase --with-coverage


.. On Windows:

Windows の場合:


.. code-block:: text

   c:\pyramidtut\tutorial> ..\Scripts\nosetests --cover-package=tutorial ^
         --cover-erase --with-coverage


.. If successful, you will see output something like this:

成功すると、次のような出力が見られるでしょう:


::

  .
  Name               Stmts   Miss  Cover   Missing
  ------------------------------------------------
  tutorial              11      7    36%   9-15
  tutorial.models       17      0   100%   
  tutorial.scripts       0      0   100%   
  tutorial.tests        24      0   100%   
  tutorial.views         6      0   100%   
  ------------------------------------------------
  TOTAL                 58      7    88%   
  ----------------------------------------------------------------------
  Ran 1 test in 0.459s

  OK


.. Looks like our package doesn't quite have 100% test coverage.

私たちのパッケージは 100% のテストカバレッジではないようです。


.. Initializing the Database

データベースの初期化
=========================

.. We need to use the ``initialize_tutorial_db`` :term:`console
.. script` to initialize our database.

データベースを初期化するために ``initialize_tutorial_db``
:term:`console script` を使用する必要があります。


.. Type the following command, make sure you are still in the ``tutorial``
.. directory (the directory with a ``development.ini`` in it):

``tutorial`` ディレクトリ (``development.ini`` のあるディレクトリ) の中
にいることを確認して、次のコマンドをタイプしてください:


.. On UNIX:

UNIX の場合:


.. code-block:: text

   $ ../bin/initialize_tutorial_db development.ini


.. On Windows:

Windows の場合:


.. code-block:: text

   c:\pyramidtut\tutorial> ..\Scripts\initialize_tutorial_db development.ini


.. The output to your console should be something like this:

コンソールに対して次のような出力が行われるはずです:


::

  2011-11-26 14:42:25,012 INFO  [sqlalchemy.engine.base.Engine][MainThread] 
                                PRAGMA table_info("models")
  2011-11-26 14:42:25,013 INFO  [sqlalchemy.engine.base.Engine][MainThread] ()
  2011-11-26 14:42:25,013 INFO  [sqlalchemy.engine.base.Engine][MainThread] 
  CREATE TABLE models (
  	id INTEGER NOT NULL, 
  	name VARCHAR(255), 
  	value INTEGER, 
  	PRIMARY KEY (id), 
  	UNIQUE (name)
  )
  2011-11-26 14:42:25,013 INFO  [sqlalchemy.engine.base.Engine][MainThread] ()
  2011-11-26 14:42:25,135 INFO  [sqlalchemy.engine.base.Engine][MainThread] 
                                COMMIT
  2011-11-26 14:42:25,137 INFO  [sqlalchemy.engine.base.Engine][MainThread] 
                                BEGIN (implicit)
  2011-11-26 14:42:25,138 INFO  [sqlalchemy.engine.base.Engine][MainThread] 
                                INSERT INTO models (name, value) VALUES (?, ?)
  2011-11-26 14:42:25,139 INFO  [sqlalchemy.engine.base.Engine][MainThread] 
                                (u'one', 1)
  2011-11-26 14:42:25,140 INFO  [sqlalchemy.engine.base.Engine][MainThread] 
                                COMMIT


.. Success!  You should now have a ``tutorial.sqlite`` file in your current working
.. directory.  This will be a SQLite database with a single table defined in it
.. (``models``).

成功です! 現在の作業ディレクトリに ``tutorial.sqlite`` ファイルができている
はずです。このファイルは、単一のテーブルが定義された SQLite データベース
(``models``) になります。


.. Starting the Application

.. _wiki2-start-the-application:

アプリケーションの起動
==========================

.. Start the application.

アプリケーションを起動します。


.. On UNIX:

UNIX の場合:


.. code-block:: text

   $ ../bin/pserve development.ini --reload


.. On Windows:

Windows の場合:


.. code-block:: text

   c:\pyramidtut\tutorial> ..\Scripts\pserve development.ini --reload


.. If successful, you will see something like this on your console:

成功すると、コンソールに次のような出力が見られるでしょう:


::

  Starting subprocess with file monitor
  Starting server in PID 8966.
  Starting HTTP server on http://0.0.0.0:6543


.. This means the server is ready to accept requests.

これは、サーバーがリクエストを受け付ける準備ができていることを意味します。


.. At this point, when you visit ``http://localhost:6543/`` in your web browser,
.. you will see the generated application's default page.

この時点で、ウェブブラウザで ``http://localhost:6543/`` を開くと、
生成されたアプリケーションのデフォルトページが表示されるでしょう。


.. One thing you'll notice is the "debug toolbar" icon on right hand side of the
.. page.  You can read more about the purpose of the icon at
.. :ref:`debug_toolbar`.  It allows you to get information about your
.. application while you develop.

ページの右側に "デバッグツールバー" アイコンがあるのに気が付くと思います。
アイコンの用途について詳しいことは :ref:`debug_toolbar` で読むことができます。
それは開発中にアプリケーションに関する情報を取得することができます。


.. Decisions the ``alchemy`` Scaffold Has Made For You

``alchemy`` scaffold が決めてくれること
=================================================================

.. Creating a project using the ``alchemy`` scaffold makes
.. the following assumptions:

``alchemy`` scaffold を使ってプロジェクトを作成すると、次のことが仮定されます:


.. - you are willing to use :term:`SQLAlchemy` as a database access tool

- データベースアクセスツールとして :term:`SQLAlchemy` を使用する


.. - you are willing to use :term:`url dispatch` to map URLs to code.

- URLからコードへのマッピング方法に :term:`url dispatch` を使用する


.. - you want to use ``ZopeTransactionExtension`` and ``pyramid_tm`` to scope
..   sessions to requests

- セッションをリクエストスコープにするために
  ``ZopeTransactionExtension`` と ``pyramid_tm`` を使用する


.. .. note::

..    :app:`Pyramid` supports any persistent storage mechanism (e.g. object
..    database or filesystem files, etc).  It also supports an additional
..    mechanism to map URLs to code (:term:`traversal`).  However, for the
..    purposes of this tutorial, we'll only be using url dispatch and
..    SQLAlchemy.

.. note::

   :app:`Pyramid` は任意の永続化ストレージ機構をサポートしています
   (例えば、オブジェクトデータベースやファイルシステムなど)。
   また、URLからコードへのマッピングを行う追加の方法もサポートします
   (:term:`traversal`)。 しかし、このチュートリアルの用途には
   url dispatch と SQLAlchemy だけを使用します。
