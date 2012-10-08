.. Adding Tests

================
テストを追加する
================

.. We will now add tests for the models and the views and a few functional
.. tests in the ``tests.py``.  Tests ensure that an application works, and
.. that it continues to work after changes are made in the future.

これから ``tests.py`` にモデル、ビューおよびいくつかの機能テストを追加します。
テストは、アプリケーションが動作すること、そして、将来的にいくつかの
変更が行われた後でも正常に動き続けていることを確認します。


.. The source code for this tutorial stage can be browsed at
.. `http://github.com/Pylons/pyramid/tree/1.3-branch/docs/tutorials/wiki2/src/tests/
.. <http://github.com/Pylons/pyramid/tree/1.3-branch/docs/tutorials/wiki2/src/tests/>`_.

このチュートリアルステージのソースコードを以下の場所で閲覧することができます。
`http://github.com/Pylons/pyramid/tree/1.3-branch/docs/tutorials/wiki2/src/tests/
<http://github.com/Pylons/pyramid/tree/1.3-branch/docs/tutorials/wiki2/src/tests/>`_.


.. Testing the Models

モデルのテスト
==================

.. To test the model class ``Page`` we'll add a new ``PageModelTests``
.. class to our ``tests.py`` file that was generated as part of the
.. ``alchemy`` scaffold.

モデルクラス ``Page`` をテストするために、 ``alchemy`` scaffold の一部
として生成された ``tests.py`` ファイルに、新しい ``PageModelTests``
クラスを追加します。


.. Testing the Views

ビューのテスト
=================

.. We'll modify our ``tests.py`` file, adding tests for each view
.. function we added above.  As a result, we'll *delete* the
.. ``ViewTests`` class that the ``alchemy`` scaffold provided, and add
.. four other test classes: ``ViewWikiTests``, ``ViewPageTests``,
.. ``AddPageTests``, and ``EditPageTests``.  These test the
.. ``view_wiki``, ``view_page``, ``add_page``, and ``edit_page`` views
.. respectively.

追加した各ビュー関数に対するテストを加えるために ``tests.py`` ファイル
を修正します。その結果、 ``alchemy`` scaffold の提供している ``ViewTests``
テストを *削除* して、他の4つのテストクラスを追加します: ``ViewWikiTests``,
``ViewPageTests``, ``AddPageTests``, ``EditPageTests`` です。これらは
``view_wiki``, ``view_page``, ``add_page``, ``edit_page`` ビューをそれぞれ
テストします。


.. Functional tests

機能テスト
================

.. We'll test the whole application, covering security aspects that are not
.. tested in the unit tests, like logging in, logging out, checking that
.. the ``viewer`` user cannot add or edit pages, but the ``editor`` user
.. can, and so on.

ユニットテストではテストされない、セキュリティの側面を含む
アプリケーション全体をテストします。ログイン、ログアウト、
``viewer`` ユーザはページを追加したり編集したりできないが
``editor`` ユーザはできることのチェック、などです。


.. Viewing the results of all our edits to ``tests.py``

``tests.py`` に対するすべての編集結果を見る
====================================================

.. Once we're done with the ``tests.py`` module, it will look a lot like:

``tests.py`` モジュールが完成すると, 次のようになります:


.. literalinclude:: src/tests/tutorial/tests.py
   :linenos:
   :language: python


.. Running the Tests

テストの実行
=================

.. We can run these tests by using ``setup.py test`` in the same way we did in
.. :ref:`running_tests`.  However, first we must edit our ``setup.py`` to
.. include a dependency on WebTest, which we've used in our ``tests.py``.
.. Change the ``requires`` list in ``setup.py`` to include ``WebTest``.

:ref:`running_tests` の中でやったのと同じ方法で、 ``setup.py test`` を
使用してこれらのテストを実行できます。しかし、最初に WebTest に対する
依存性を含めるために ``setup.py`` を編集しなければなりません。
``tests.py`` の中でそれを使用しているからです。 ``WebTest`` を含めるために
``setup.py`` の中の ``requires`` リストを変更してください。


.. literalinclude:: src/tests/setup.py
   :linenos:
   :language: python
   :lines: 9-20
   :emphasize-lines: 10


.. After we've added a dependency on WebTest in ``setup.py``, we need to rerun
.. ``setup.py develop`` to get WebTest installed into our virtualenv.  Assuming
.. our shell's current working directory is the "tutorial" distribution
.. directory:

``setup.py`` に WebTest に対する依存性を加えた後で、 virtualenv に
WebTest をインストールするために ``setup.py develop`` を再実行する必要
があります。シェルの現在の作業ディレクトリが "tutorial" と仮定して:


.. On UNIX:

UNIX の場合:


.. code-block:: text

   $ ../bin/python setup.py develop


.. On Windows:

Windows の場合:


.. code-block:: text

   c:\pyramidtut\tutorial> ..\Scripts\python setup.py develop


.. Once that command has completed successfully, we can run the tests
.. themselves:

コマンドが正常に実行されたら、テスト自体を実行することができます:


.. On UNIX:

UNIX の場合:


.. code-block:: text

   $ ../bin/python setup.py test -q


.. On Windows:

Windows の場合:


.. code-block:: text

   c:\pyramidtut\tutorial> ..\Scripts\python setup.py test -q


.. The expected result ends something like:

期待される出力はこのようになります:


.. code-block:: text

   ......................
   ----------------------------------------------------------------------
   Ran 21 tests in 2.700s

   OK
