.. Defining the Domain Model

=========================
ドメインモデルを定義する
=========================

.. The first change we'll make to our stock ``pcreate``-generated application will
.. be to define a :term:`domain model` constructor representing a wiki page.
.. We'll do this inside our ``models.py`` file.

``pcreate`` から生成したアプリケーションに対して最初に行う変更は、 wiki ページ
を表す :term:`domain model` のコンストラクタを定義することです。これを
``models.py`` ファイルの中で行います。


.. The source code for this tutorial stage can be browsed at
.. `http://github.com/Pylons/pyramid/tree/1.3-branch/docs/tutorials/wiki2/src/models/
.. <http://github.com/Pylons/pyramid/tree/1.3-branch/docs/tutorials/wiki2/src/models/>`_.

このチュートリアルステージのソースコードを以下の場所で閲覧することができます。
`http://github.com/Pylons/pyramid/tree/1.3-branch/docs/tutorials/wiki2/src/models/
<http://github.com/Pylons/pyramid/tree/1.3-branch/docs/tutorials/wiki2/src/models/>`_.


.. Making Edits to ``models.py``

``models.py`` を編集する
-----------------------------

.. .. note::

..   There is nothing special about the filename ``models.py``.  A
..   project may have many models throughout its codebase in arbitrarily-named
..   files.  Files implementing models often have ``model`` in their filenames
..   (or they may live in a Python subpackage of your application package named
..   ``models``) , but this is only by convention.

.. note::

   ``models.py`` というファイル名には特別な
   意味はありません。プロジェクトはそのコードベース全体で任意の名前の
   ファイル中に多くのモデルを持つことができます。モデルを実装するファイルは
   多くの場合 ``model`` というファイル名を持っています (もしくは
   ``models`` という名前のアプリケーションパッケージの中の Python サブ
   パッケージに存在しています) が、これは単なる慣習です。


.. Open ``tutorial/tutorial/models.py`` file and edit it to look like the 
.. following:

``tutorial/tutorial/models.py`` ファイルを開き、以下のように編集してください:


.. literalinclude:: src/models/tutorial/models.py
   :linenos:
   :language: py
   :emphasize-lines: 20-22,25,27,29


.. (The highlighted lines are the ones that need to be changed.)

(ハイライトされた行は変更が必要な箇所です)


.. The first thing we've done is to do is remove the stock ``MyModel`` class
.. from the generated ``models.py`` file.  The ``MyModel`` class is only a
.. sample and we're not going to use it.

初めにしたことは、生成された ``models.py`` ファイルから ``MyModel``
クラスを削除することです。 ``MyModel`` クラスは単なるサンプルで、
使うことはありません。


.. Then, we added a ``Page`` class.  Because this is a SQLAlchemy application,
.. this class inherits from an instance of
.. :class:`sqlalchemy.ext.declarative.declarative_base`.

それから、 ``Page`` クラスを追加しました。これは SQLAlchemy アプリケーション
なので、このクラスは
:class:`sqlalchemy.ext.declarative.declarative_base` のインスタンスから
継承します。


.. literalinclude:: src/models/tutorial/models.py
   :pyobject: Page
   :linenos:
   :language: python


.. As you can see, our ``Page`` class has a class level attribute
.. ``__tablename__`` which equals the string ``'pages'``.  This means that
.. SQLAlchemy will store our wiki data in a SQL table named ``pages``.  Our
.. ``Page`` class will also have class-level attributes named ``id``, ``name`` and
.. ``data`` (all instances of :class:`sqlalchemy.Column`).  These will map to
.. columns in the ``pages`` table.  The ``id`` attribute will be the primary key
.. in the table.  The ``name`` attribute will be a text attribute, each value of
.. which needs to be unique within the column.  The ``data`` attribute is a text
.. attribute that will hold the body of each page.

ご覧の通り、 ``Page`` クラスはクラス属性 ``__tablename__`` を持っていて、
その値は文字列 ``'pages'`` に等しいです。これが意味するのは、
SQLAlchemy が wiki データを ``pages`` という名前の SQL テーブルに保存
するということです。 ``Page`` クラスはまた、 ``id``, ``name``, ``data`` という
名前のクラス属性を持っています (全て :class:`sqlalchemy.Column` の
インスタンスです)。これらは ``pages`` テーブルのカラムにマップされます。
``id`` 属性はテーブルのプライマリキーになります。 ``name`` 属性はテキスト
属性で、各値はカラム内で一意である必要があります。 ``data`` 属性は
各ページの本体を保持するテキスト属性です。


.. Changing ``scripts/initializedb.py``

``scripts/initializedb.py`` を変更する
--------------------------------------

.. We haven't looked at the details of this file yet, but within the ``scripts``
.. directory of your ``tutorial`` package is a file named ``initializedb.py``.  Code
.. in this file is executed whenever we run the ``initialize_tutorial_db`` command
.. (as we did in the installation step of this tutorial).

このファイルの詳細はまだ見ていませんが、 ``tutorial`` パッケージの
``scripts`` ディレクトリ内に ``initializedb.py`` という名前のファイルが
あります。このファイルに含まれるコードは、 ``initialize_tutorial_db``
コマンドを実行したときに常に実行されます (このチュートリアルのインストール
ステップで行ったように)。


.. Since we've changed our model, we need to make changes to our ``initializedb.py``
.. script.  In particular, we'll replace our import of ``MyModel`` with one of
.. ``Page`` and we'll change the very end of the script to create a ``Page``
.. rather than a ``MyModel`` and add it to our ``DBSession``.

モデルを変更したので、 ``initializedb.py`` スクリプトに変更を加える必要が
あります。特に、 ``MyModel`` のインポートを ``Page`` のうちの1つに置き換え
ます。また、 ``MyModel`` ではなく ``Page`` を作成し、かつ ``DBSession``
にそれを加えるためにスクリプトの最終行を変更します。


.. Open ``tutorial/tutorial/scripts/initializedb.py`` and edit it to look like the 
.. following:

``tutorial/tutorial/scripts/initializedb.py`` ファイルを開き、以下のように
編集してください:


.. literalinclude:: src/models/tutorial/scripts/initializedb.py
   :linenos:
   :language: python
   :emphasize-lines: 14,36


.. (Only the highlighted lines need to be changed.)

(ハイライトされた行は変更が必要な箇所です)


.. Reinitializing the Database

データベースの再初期化
---------------------------

.. Because our model has changed, in order to reinitialize the database, we need
.. to rerun the ``initialize_tutorial_db`` command to pick up the changes you've made
.. to both the models.py file and to the initializedb.py file.  From the root of the
.. ``tutorial`` project, directory execute the following commands.

モデルが変わったので、データベースを再初期化するために、
``initialize_tutorial_db`` コマンドを再実行して models.py ファイルと
initializedb.py ファイルの両方に加えた変更をピックアップする必要があります。
``tutorial`` プロジェクトのルートディレクトリで次のコマンドを実行します。


.. On UNIX:

UNIX の場合:


.. code-block:: text

   $ ../bin/initialize_tutorial_db development.ini


.. On Windows:

Windows の場合:


.. code-block:: text

   c:\pyramidtut\tutorial> ..\Scripts\initialize_tutorial_db development.ini


.. Success will look something like this:

成功すると以下のような出力が行われます:


::

  2011-11-27 01:22:45,277 INFO  [sqlalchemy.engine.base.Engine][MainThread] 
                                PRAGMA table_info("pages")
  2011-11-27 01:22:45,277 INFO  [sqlalchemy.engine.base.Engine][MainThread] ()
  2011-11-27 01:22:45,277 INFO  [sqlalchemy.engine.base.Engine][MainThread] 
  CREATE TABLE pages (
  	id INTEGER NOT NULL, 
  	name TEXT, 
  	data TEXT, 
  	PRIMARY KEY (id), 
  	UNIQUE (name)
  )


  2011-11-27 01:22:45,278 INFO  [sqlalchemy.engine.base.Engine][MainThread] ()
  2011-11-27 01:22:45,397 INFO  [sqlalchemy.engine.base.Engine][MainThread] 
                                COMMIT
  2011-11-27 01:22:45,400 INFO  [sqlalchemy.engine.base.Engine][MainThread] 
                                BEGIN (implicit)
  2011-11-27 01:22:45,401 INFO  [sqlalchemy.engine.base.Engine][MainThread] 
                                INSERT INTO pages (name, data) VALUES (?, ?)
  2011-11-27 01:22:45,401 INFO  [sqlalchemy.engine.base.Engine][MainThread] 
                                ('FrontPage', 'This is the front page')
  2011-11-27 01:22:45,402 INFO  [sqlalchemy.engine.base.Engine][MainThread] 
                                COMMIT


.. Viewing the Application in a Browser

ブラウザでアプリケーションを表示する
------------------------------------

.. We can't.  At this point, our system is in a "non-runnable" state; we'll need
.. to change view-related files in the next chapter to be able to start the
.. application successfully.  If you try to start the application (See
.. :ref:`wiki2-start-the-application`), you'll wind
.. up with a Python traceback on your console that ends with this exception:

できません。この時点ではシステムは "実行不可能な" 状態です。次の章で
ビュー関連のファイルを変更すればアプリケーションが正常に起動するように
なります。もしアプリケーションを起動しようとすると
(:ref:`wiki2-start-the-application` 参照)、コンソール上にPython
トレースバックが表示されて終了することになるでしょう。トレースバックの
最後は以下のような例外になります:


.. code-block:: text

   ImportError: cannot import name MyModel


.. This will also happen if you attempt to run the tests.

これはまた、テストを実行しようとした場合にも起こります。
