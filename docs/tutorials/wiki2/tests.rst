.. Adding Tests

================
テストを追加する
================

.. We will now add tests for the models and the views and a few functional
.. tests in the ``tests.py``.  Tests ensure that an application works, and
.. that it continues to work after changes are made in the future.

今から ``tests.py`` にモデル、ビューおよびいくつかの機能テストを追加します。
テストでは、アプリケーションが動作すること、そして、いくつかの変更が将来的に行った後でも正常に動作し続けることを確認します。


The source code for this tutorial stage can be browsed at
`http://github.com/Pylons/pyramid/tree/1.3-branch/docs/tutorials/wiki2/src/tests/
<http://github.com/Pylons/pyramid/tree/1.3-branch/docs/tutorials/wiki2/src/tests/>`_.


.. Testing the Models

モデルのテスト
==================

To test the model class ``Page`` we'll add a new ``PageModelTests``
class to our ``tests.py`` file that was generated as part of the
``alchemy`` scaffold.

.. Testing the Views

ビューのテスト
=================

We'll modify our ``tests.py`` file, adding tests for each view
function we added above.  As a result, we'll *delete* the
``ViewTests`` class that the ``alchemy`` scaffold provided, and add
four other test classes: ``ViewWikiTests``, ``ViewPageTests``,
``AddPageTests``, and ``EditPageTests``.  These test the
``view_wiki``, ``view_page``, ``add_page``, and ``edit_page`` views
respectively.


.. Functional tests

機能テスト
================

We'll test the whole application, covering security aspects that are not
tested in the unit tests, like logging in, logging out, checking that
the ``viewer`` user cannot add or edit pages, but the ``editor`` user
can, and so on.


.. Viewing the results of all our edits to ``tests.py``

``tests.py`` の私たちが編集した全ての結果を見る
====================================================

Once we're done with the ``tests.py`` module, it will look a lot like:

.. literalinclude:: src/tests/tutorial/tests.py
   :linenos:
   :language: python


.. Running the Tests

テストの実行
=================

We can run these tests by using ``setup.py test`` in the same way we did in
:ref:`running_tests`.  However, first we must edit our ``setup.py`` to
include a dependency on WebTest, which we've used in our ``tests.py``.
Change the ``requires`` list in ``setup.py`` to include ``WebTest``.

.. literalinclude:: src/tests/setup.py
   :linenos:
   :language: python
   :lines: 9-20
   :emphasize-lines: 10

After we've added a dependency on WebTest in ``setup.py``, we need to rerun
``setup.py develop`` to get WebTest installed into our virtualenv.  Assuming
our shell's current working directory is the "tutorial" distribution
directory:

On UNIX:

.. code-block:: text

   $ ../bin/python setup.py develop

On Windows:

.. code-block:: text

   c:\pyramidtut\tutorial> ..\Scripts\python setup.py develop

Once that command has completed successfully, we can run the tests
themselves:

On UNIX:

.. code-block:: text

   $ ../bin/python setup.py test -q

On Windows:

.. code-block:: text

   c:\pyramidtut\tutorial> ..\Scripts\python setup.py test -q

The expected result ends something like:

.. code-block:: text

   ......................
   ----------------------------------------------------------------------
   Ran 21 tests in 2.700s

   OK
