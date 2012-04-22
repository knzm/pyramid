.. Defining the Domain Model

=========================
ドメインモデルを定義する
=========================

.. The first change we'll make to our stock pcreate-generated application will
.. be to define a :term:`domain model` constructor representing a wiki page.
.. We'll do this inside our ``models.py`` file.

最初の変更はwikiページを表す :term:`domain model` のコンストラクタの定義をpcreateから生成したアプリケーションに作ることです。
これを ``models.py`` ファイルの中で行います。


.. The source code for this tutorial stage can be browsed at
.. `http://github.com/Pylons/pyramid/tree/1.3-branch/docs/tutorials/wiki2/src/models/
.. <http://github.com/Pylons/pyramid/tree/1.3-branch/docs/tutorials/wiki2/src/models/>`_.

このチュートリアルステージのソースコードを閲覧することができます。
`http://github.com/Pylons/pyramid/tree/1.3-branch/docs/tutorials/wiki2/src/models/
<http://github.com/Pylons/pyramid/tree/1.3-branch/docs/tutorials/wiki2/src/models/>`_.


.. Making Edits to ``models.py``

``models.py`` の編集をする
-----------------------------

.. .. note::

..   There is nothing automagically special about the filename ``models.py``.  A
..   project may have many models throughout its codebase in arbitrarily-named
..   files.  Files implementing models often have ``model`` in their filenames
..   (or they may live in a Python subpackage of your application package named
..   ``models``) , but this is only by convention.

.. note::

   ``models.py`` のファイル名は自動的に付けられ、特別な意味はありません。
   プロジェクトは任意の名前のファイルにそのコードベース全体の多くのモデルを持っているかもしれません。
   モデルを実装するファイルは多くの場合 ``model`` というファイル名を持っています
   (もしくはあなたの ``models`` アプリケーションパッケージのPythonのサブパッケージにいます)が、これは慣例です。


Open ``tutorial/tutorial/models.py`` file and edit it to look like the 
following:


.. literalinclude:: src/models/tutorial/models.py
   :linenos:
   :language: py
   :emphasize-lines: 19-21,24,26,28


(The highlighted lines are the ones that need to be changed.)


.. The first thing we've done is to do is remove the stock ``MyModel`` class
.. from the generated ``models.py`` file.  The ``MyModel`` class is only a
.. sample and we're not going to use it.

初めにしたことは生成された ``models.py`` ファイルから ``MyModel`` クラスを削除することです。
``MyModel`` クラスはサンプルだけで、それを使うつもりはありません。


.. Then, we added a ``Page`` class.  Because this is a SQLAlchemy application,
.. this class inherits from an instance of
.. :class:`sqlalchemy.ext.declarative.declarative_base`.

それから、 ``Page`` クラスを追加しました。これはSQLAlchemyアプリケーションなので、
このクラスは :class:`sqlalchemy.ext.declarative.declarative_base` のインスタンスから継承します。


.. literalinclude:: src/models/tutorial/models.py
   :pyobject: Page
   :linenos:
   :language: python


.. As you can see, our ``Page`` class has a class level attribute
.. ``__tablename__`` which equals the string ``'pages'``.  This means that
.. SQLAlchemy will store our wiki data in a SQL table named ``pages``.  Our Page
.. class will also have class-level attributes named ``id``, ``name`` and
.. ``data`` (all instances of :class:`sqlalchemy.Column`).  These will map to
.. columns in the ``pages`` table.  The ``id`` attribute will be the primary key
.. in the table.  The ``name`` attribute will be a text attribute, each value of
.. which needs to be unique within the column.  The ``data`` attribute is a text
.. attribute that will hold the body of each page.

ご覧の通り、私たちの ``Page`` クラスは文字列 ``pages`` と等しいクラス属性 ``__tablename__`` を持っています。
これが意味するのはSQLAlchemyが ``pages`` という名前のSQLのテーブルに私たちのwikiのデータをストアするということです。
Pageクラスはまた、 ``id`` 、 ``name`` および ``data`` という名前のクラス属性を持っています。(全て :class:`sqlalchemy.Column` のインスタンス)
これらは ``pages`` テーブルのカラムにマップされます。
``id`` 属性はテーブルのプライマリキーになります。
``name`` 属性はテキスト属性になります。
各値はカラム内で一意である必要があります。
``data`` 属性は各ページの本体を保持するテキスト属性です。


Changing ``scripts/initializedb.py``
------------------------------------

We haven't looked at the details of this file yet, but within the ``scripts``
directory of your ``tutorial`` package is a file named ``initializedb.py``.  Code
in this file is executed whenever we run the ``initialize_tutorial_db`` command
(as we did in the installation step of this tutorial).

Since we've changed our model, we need to make changes to our ``initializedb.py``
script.  In particular, we'll replace our import of ``MyModel`` with one of
``Page`` and we'll change the very end of the script to create a ``Page``
rather than a ``MyModel`` and add it to our ``DBSession``.

Open ``tutorial/tutorial/scripts/initializedb.py`` and edit it to look like the 
following:

.. literalinclude:: src/models/tutorial/scripts/initializedb.py
   :linenos:
   :language: python
   :emphasize-lines: 14,34

(Only the highlighted lines need to be changed.)

Reinitializing the Database
---------------------------

Because our model has changed, in order to reinitialize the database, we need
to rerun the ``initialize_tutorial_db`` command to pick up the changes you've made
to both the models.py file and to the initializedb.py file.  From the root of the
``tutorial`` project, directory execute the following commands.

On UNIX:

.. code-block:: text

   $ ../bin/initialize_tutorial_db development.ini

On Windows:

.. code-block:: text

   c:\pyramidtut\tutorial> ..\Scripts\initialize_tutorial_db development.ini

Success will look something like this::

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

ブラウザでアプリケーションを見る
------------------------------------

.. We can't.  At this point, our system is in a "non-runnable" state; we'll need
.. to change view-related files in the next chapter to be able to start the
.. application successfully.  If you try to start the application (See
.. :ref:`wiki2-start-the-application`), you'll wind
.. up with a Python traceback on your console that ends with this exception:

できません。この時点では私たちのシステムは "実行不可能な" 状態です。
私たちは次の章でビュー関連のファイルを変更すればアプリケーションの起動に成功できます。
もしあなたがアプリケーションを起動しようとすると (:ref:`wiki2-start-the-application` 参照)、この例外で終了し、あなたのコンソール上でPythonトレースバックで終了することになるでしょう。


.. code-block:: text

   ImportError: cannot import name MyModel


.. This will also happen if you attempt to run the tests.

これはまた、テストを実行しようとした場合にも起こります。
