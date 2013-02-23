.. index::
   single: unit testing
   single: integration testing
   single: functional testing

.. Unit, Integration, and Functional Testing

.. _testing_chapter:

単体テスト、結合テスト、機能テスト
=========================================

.. *Unit testing* is, not surprisingly, the act of testing a "unit" in your
.. application.  In this context, a "unit" is often a function or a method of a
.. class instance.  The unit is also referred to as a "unit under test".

*単体テスト (unit testing)* は、読んで字のごとく、アプリケーションの中の
「ユニット (単位)」をテストする行為です。この文脈で「ユニット」とは、多くの
場合関数またはクラスインスタンスのメソッドです。ユニットは「テスト単位
(unit under test)」とも呼ばれます。


.. The goal of a single unit test is to test **only** some permutation of the
.. "unit under test".  If you write a unit test that aims to verify the result
.. of a particular codepath through a Python function, you need only be
.. concerned about testing the code that *lives in the function body itself*.
.. If the function accepts a parameter that represents a complex application
.. "domain object" (such as a resource, a database connection, or an SMTP
.. server), the argument provided to this function during a unit test *need not
.. be* and likely *should not be* a "real" implementation object.  For example,
.. although a particular function implementation may accept an argument that
.. represents an SMTP server object, and the function may call a method of this
.. object when the system is operating normally that would result in an email
.. being sent, a unit test of this codepath of the function does *not* need to
.. test that an email is actually sent.  It just needs to make sure that the
.. function calls the method of the object provided as an argument that *would*
.. send an email if the argument happened to be the "real" implementation of an
.. SMTP server object.

単一の単体テストのゴールは、テスト単位の特定の組み合わせ *のみ*
をテストすることです。 Python 関数によって特定のコードパスの結果を確認
することを目標とする単体テストを書く場合、 *関数本体それ自体に含まれる*
コードのテストについてのみ関心を払う必要があります。関数が (リソース、
データベース接続あるいは SMTP サーバーのような) 複雑なアプリケーション
「ドメインオブジェクト」を表わすパラメータを受け付ける場合、単体テスト
の間この関数に提供される引数は、「本物の」実装オブジェクト *である必要
はなく* 、そしておそらく *そうであるべきではありません* 。例えば、ある
関数の実装が SMTP サーバーオブジェクトを表わす引数を受け取り、システム
が通常動作している場合にこのオブジェクトのメソッドを呼ぶことによって電子
メールが送られるかもしれませんが、この関数のこのコードパスの単体テストは、
電子メールが実際に送られることをテストする必要は *ありません* 。
それは、単に引数として渡されたオブジェクト (もしその引数が SMTP サーバー
オブジェクトの「実際の」実装であったなら、電子メールを送った *だろう*
オブジェクト) のメソッドをその関数が呼ぶことを確かめる必要があるだけです。


.. An *integration test*, on the other hand, is a different form of testing in
.. which the interaction between two or more "units" is explicitly tested.
.. Integration tests verify that the components of your application work
.. together.  You *might* make sure that an email was actually sent in an
.. integration test.

*結合テスト (integration test)* は、それに対して、2つ以上の「ユニット」間の
相互作用が明示的にテストされる、テストの異なる形式です。結合テストは、
アプリケーションの複数のコンポーネントが一緒に動くことを確認します。
結合テストの中で電子メールが実際に送られたことを確かめることは *ありえます* 。


.. A *functional test* is a form of integration test in which the application is
.. run "literally".  You would *have to* make sure that an email was actually
.. sent in a functional test, because it tests your code end to end.

*機能テスト (functional test)* は、アプリケーションが「文字通りに」実行される
結合テストの一形式です。機能テストでは電子メールが実際に送られることを
確かめ *なければならない* でしょう。なぜならそれはコードを端から端まで
テストするからです。


.. It is often considered best practice to write each type of tests for any
.. given codebase.  Unit testing often provides the opportunity to obtain better
.. "coverage": it's usually possible to supply a unit under test with arguments
.. and/or an environment which causes *all* of its potential codepaths to be
.. executed.  This is usually not as easy to do with a set of integration or
.. functional tests, but integration and functional testing provides a measure of
.. assurance that your "units" work together, as they will be expected to when
.. your application is run in production.

いかなるコードベースに対しても、それぞれの種類のテストを書くことは、
しばしばベストプラクティスと考えられます。単体テストは、しばしばより良い
「カバレッジ」を得る機会を提供します: テスト単位に、その *すべての*
潜在的なコードパスを実行する引数や環境を提供することは通常可能です。
結合テストや機能テストでこれを行うことは通常容易ではありませんが、
結合テストと機能テストは「ユニット」が一緒に動作するという保証 (それは
アプリケーションがプロダクション環境で実行されるときに期待されることです)
の measure (基準, 評価, 手段) を提供します。


.. The suggested mechanism for unit and integration testing of a :app:`Pyramid`
.. application is the Python :mod:`unittest` module.  Although this module is
.. named :mod:`unittest`, it is actually capable of driving both unit and
.. integration tests.  A good :mod:`unittest` tutorial is available within `Dive
.. Into Python <http://www.diveintopython.net/unit_testing/index.html>`_ by Mark
.. Pilgrim.

:app:`Pyramid` アプリケーションで推奨されているユニットテストおよび結合
テストのメカニズムは、 Python の :mod:`unittest` モジュールです。
このモジュールは :mod:`unittest` という名前ですが、実際には単体テストと
結合テストの両方を実行することができます。単体テストの良いチュートリアルは
Mark Pilgrim による `Dive Into Python
<http://www.diveintopython.net/unit_testing/index.html>`_ にあります。


.. :app:`Pyramid` provides a number of facilities that make unit, integration,
.. and functional tests easier to write.  The facilities become particularly
.. useful when your code calls into :app:`Pyramid` -related framework functions.

:app:`Pyramid` は、単体テスト、結合テスト、機能テストを簡単に書けるように
するための能力を多く提供しています。その能力は、コードが :app:`Pyramid` 関連の
フレームワーク機能を呼び出す場合に特に有用になります。


.. index::
   single: test setup
   single: test tear down
   single: unittest

.. Test Set Up and Tear Down

.. _test_setup_and_teardown:

テストの set up と tear down
----------------------------

.. :app:`Pyramid` uses a "global" (actually :term:`thread local`) data structure
.. to hold on to two items: the current :term:`request` and the current
.. :term:`application registry`.  These data structures are available via the
.. :func:`pyramid.threadlocal.get_current_request` and
.. :func:`pyramid.threadlocal.get_current_registry` functions, respectively.
.. See :ref:`threadlocals_chapter` for information about these functions and the
.. data structures they return.

:app:`Pyramid` は、現在の :term:`request` と現在の
:term:`application registry` という 2 つの要素を保持するために「グローバル」
(実際には :term:`thread local`) なデータ構造を使用します。これらのデータ
構造はそれぞれ :func:`pyramid.threadlocal.get_current_request` 関数と
:func:`pyramid.threadlocal.get_current_registry` 関数で得ることが可能です。
これらの関数、およびそれらが返すデータ構造に関する情報については、
:ref:`threadlocals_chapter` を参照してください。


.. If your code uses these ``get_current_*`` functions or calls :app:`Pyramid`
.. code which uses ``get_current_*`` functions, you will need to call
.. :func:`pyramid.testing.setUp` in your test setup and you will need to call
.. :func:`pyramid.testing.tearDown` in your test teardown.
.. :func:`~pyramid.testing.setUp` pushes a registry onto the :term:`thread
.. local` stack, which makes the ``get_current_*`` functions work.  It returns a
.. :term:`Configurator` object which can be used to perform extra configuration
.. required by the code under test.  :func:`~pyramid.testing.tearDown` pops the
.. thread local stack.

あなたのコードがこれらの ``get_current_*`` 関数を使用するか、
``get_current_*`` 関数を使用する :app:`Pyramid` コードを呼んでいるなら、
テストの setup で :func:`pyramid.testing.setUp` を呼び、テストの
teardown で :func:`~pyramid.testing.tearDown` を呼ぶ必要があるでしょう。
:func:`pyramid.testing.setUp` はレジストリを :term:`thread local`
スタックに push して ``get_current_*`` 関数が動くようにします。
:func:`pyramid.testing.setUp` は、テストコードで必要な追加の設定を
行なうために使用できる :term:`Configurator` オブジェクトを返します。
:func:`~pyramid.testing.tearDown` はスレッドローカルのスタックを pop します。


.. Normally when a Configurator is used directly with the ``main`` block of
.. a Pyramid application, it defers performing any "real work" until its
.. ``.commit`` method is called (often implicitly by the
.. :meth:`pyramid.config.Configurator.make_wsgi_app` method).  The
.. Configurator returned by :func:`~pyramid.testing.setUp` is an
.. *autocommitting* Configurator, however, which performs all actions
.. implied by methods called on it immediately.  This is more convenient
.. for unit-testing purposes than needing to call
.. :meth:`pyramid.config.Configurator.commit` in each test after adding
.. extra configuration statements.

通常 Configurator が Pyramid アプリケーションの ``main`` ブロックで
直接使用される場合、 ``.commit`` メソッドが (しばしば
:meth:`pyramid.config.Configurator.make_wsgi_app` メソッドによって暗黙的に)
呼ばれるまでは「実際の仕事」の実行は延期されます。しかし、
:meth:`pyramid.testing.setUp` によって返された Configurator は、
*autocommitting* Configurator です。それに対して呼び出されるメソッドで
指示されたすべてのアクションは直ちに実行されます。単体テストを行う上で、
これは各テストに必要な設定文を追加した後に
:meth:`pyramid.config.Configurator.commit` を呼ぶ必要がないため便利です。


.. The use of the :func:`~pyramid.testing.setUp` and
.. :func:`~pyramid.testing.tearDown` functions allows you to supply each unit
.. test method in a test case with an environment that has an isolated registry
.. and an isolated request for the duration of a single test.  Here's an example
.. of using this feature:

:func:`~pyramid.testing.setUp` 関数と :func:`~pyramid.testing.tearDown`
関数を使用することで、テストケースの各単体テストメソッドに対し単一のテストに
隔離されたレジストリとリクエストが存在する環境を提供することができます。
以下は、この機能を使用する例です:


.. code-block:: python
   :linenos:

   import unittest
   from pyramid import testing

   class MyTest(unittest.TestCase):
       def setUp(self):
           self.config = testing.setUp()

       def tearDown(self):
           testing.tearDown()


.. The above will make sure that
.. :func:`~pyramid.threadlocal.get_current_registry` called within a test
.. case method of ``MyTest`` will return the :term:`application registry`
.. associated with the ``config`` Configurator instance.  Each test case
.. method attached to ``MyTest`` will use an isolated registry.

上記の例は、 ``MyTest`` のテストケースメソッド内で呼び出された
:func:`pyramid.threadlocal.get_current_registry` が ``config``
Configurator インスタンスに関連付けられた :term:`application registry`
を返すことを保証します。 ``MyTest`` に取り付けられたテストケースメソッドは、
それぞれが隔離されたレジストリを使用するようになります。


.. The :func:`~pyramid.testing.setUp` and :func:`~pyramid.testing.tearDown`
.. functions accepts various arguments that influence the environment of the
.. test.  See the :ref:`testing_module` chapter for information about the extra
.. arguments supported by these functions.

:func:`pyramid.testing.setUp` 関数と :func:`pyramid.testing.tearDown` 関数は、
テスト環境に影響を及ぼす様々な引数を受け取ります。これらの関数でサポートされる
追加の引数に関する情報については :ref:`testing_module` 章を参照してください。


.. If you also want to make :func:`~pyramid.get_current_request` return something
.. other than ``None`` during the course of a single test, you can pass a
.. :term:`request` object into the :func:`pyramid.testing.setUp` within the
.. ``setUp`` method of your test:

さらに、単一のテストの間に :func:`~pyramid.get_current_request` に
``None`` 以外の何かを返させたければ、
テストの ``setUp`` メソッド内で :func:`pyramid.testing.setUp` に
:term:`request` オブジェクトを渡すことができます:


.. code-block:: python
   :linenos:

   import unittest
   from pyramid import testing

   class MyTest(unittest.TestCase):
       def setUp(self):
           request = testing.DummyRequest()
           self.config = testing.setUp(request=request)

       def tearDown(self):
           testing.tearDown()


.. If you pass a :term:`request` object into :func:`pyramid.testing.setUp`
.. within your test case's ``setUp``, any test method attached to the
.. ``MyTest`` test case that directly or indirectly calls
.. :func:`~pyramid.threadlocal.get_current_request` will receive the request
.. object.  Otherwise, during testing,
.. :func:`~pyramid.threadlocal.get_current_request` will return ``None``.
.. We use a "dummy" request implementation supplied by
.. :class:`pyramid.testing.DummyRequest` because it's easier to construct
.. than a "real" :app:`Pyramid` request object.

テストケースの ``setUp`` 内で :func:`pyramid.testing.setUp` に
:term:`request` オブジェクトを渡した場合、
:func:`~pyramid.threadlocal.get_current_request` を直接または間接的に
呼び出す ``MyTest`` テストケースに配置された任意のテストメソッドは、
その request オブジェクトを受け取ります。そうでなければ、テスト中に
:func:`~pyramid.threadlocal.get_current_request` は ``None`` を返します。
「本物の」 :app:`Pyramid` リクエストオブジェクトを構築するより簡単なので、
ここでは :class:`pyramid.testing.DummyRequest` によって提供される「ダミー」
リクエスト実装を使用しています。


.. Test setup using a context manager

コンテキストマネージャーを使ったテストのセットアップ
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. An alternative style of setting up a test configuration is to use the
.. `with` statement and :func:`pyramid.testing.testConfig` to create a
.. context manager. The context manager will call
.. :func:`pyramid.testing.setUp` before the code under test and
.. :func:`pyramid.testing.tearDown` afterwards.

テスト設定のセットアップの別のスタイルは、コンテキストマネージャーを
作るために `with` 文と :func:`pyramid.testing.testConfig`
を使用することです。このコンテキストマネージャーは、テスト下のコードの
前に :func:`pyramid.testing.setUp` を、後で
:func:`pyramid.testing.tearDown` を呼び出します。


.. This style is useful for small self-contained tests. For example:

このスタイルは、小さな自己完結したテストで便利です。例えば:


.. code-block:: python
   :linenos:

   import unittest

   class MyTest(unittest.TestCase):

       def test_my_function(self):
           from pyramid import testing
           with testing.testConfig() as config:
               config.add_route('bar', '/bar/{id}')
               my_function_which_needs_route_bar()


What?
~~~~~

.. Thread local data structures are always a bit confusing, especially when
.. they're used by frameworks.  Sorry.  So here's a rule of thumb: if you don't
.. *know* whether you're calling code that uses the
.. :func:`~pyramid.threadlocal.get_current_registry` or
.. :func:`~pyramid.threadlocal.get_current_request` functions, or you don't care
.. about any of this, but you still want to write test code, just always call
.. :func:`pyramid.testing.setUp` in your test's ``setUp`` method and
.. :func:`pyramid.testing.tearDown` in your tests' ``tearDown`` method.  This
.. won't really hurt anything if the application you're testing does not call
.. any ``get_current*`` function.

スレッドローカルデータ構造は、特にフレームワークで使用される場合、常に
多少の混乱の元になります。残念。そこで、このような経験則があります:
:func:`~pyramid.threadlocal.get_current_registry` 関数や
:func:`~pyramid.threadlocal.get_current_request` 関数を使用するコードを
呼んでいるかどうか *知らない* 場合、あるいはそれらを気にしていないけれど
テストコードは書きたいという場合、単純にテストの ``setUp`` メソッドの
中で常に :func:`pyramid.testing.setUp` を呼び、テストの ``tearDown`` メソッド
の中で常に :func:`pyramid.testing.tearDown` を呼ぶようにしてください。
テストしているアプリケーションが ``get_current*`` 関数を呼んでいなければ、
実際にはこれはまったくの無害です。


.. index::
   single: pyramid.testing
   single: Configurator testing API

.. Using the ``Configurator`` and ``pyramid.testing`` APIs in Unit Tests

単体テストの中で ``Configurator`` と ``pyramid.testing`` API を使う
---------------------------------------------------------------------

.. The ``Configurator`` API and the ``pyramid.testing`` module provide a number
.. of functions which can be used during unit testing.  These functions make
.. :term:`configuration declaration` calls to the current :term:`application
.. registry`, but typically register a "stub" or "dummy" feature in place of the
.. "real" feature that the code would call if it was being run normally.

``Configurator`` API と ``pyramid.testing`` モジュールは、単体テストの
間に使用できる多くの機能を提供します。これらの機能は現在の
:term:`application registry` に :term:`configuration declaration` 呼び
出しを作ります。しかし、典型的には通常実行されていた場合にコードが呼び
出すだろう「実際の」機能の代わりに、「スタブ」あるいは「ダミー」の機能
を登録します。


.. For example, let's imagine you want to unit test a :app:`Pyramid` view
.. function.

例えば :app:`Pyramid` ビュー関数を単体テストしたいと想像しましょう。


.. code-block:: python
   :linenos:

   from pyramid.security import has_permission
   from pyramid.httpexceptions import HTTPForbidden

   def view_fn(request):
       if not has_permission('edit', request.context, request):
           raise HTTPForbidden
       return {'greeting':'hello'}


.. Without doing anything special during a unit test, the call to
.. :func:`~pyramid.security.has_permission` in this view function will always
.. return a ``True`` value.  When a :app:`Pyramid` application starts normally,
.. it will populate a :term:`application registry` using :term:`configuration
.. declaration` calls made against a :term:`Configurator`.  But if this
.. application registry is not created and populated (e.g. by initializing the
.. configurator with an authorization policy), like when you invoke application
.. code via a unit test, :app:`Pyramid` API functions will tend to either fail
.. or return default results.  So how do you test the branch of the code in this
.. view function that raises :exc:`HTTPForbidden`?

単体テストの間に特別なことを何もしなければ、このビュー関数内の
:func:`~pyramid.security.has_permission` の呼び出しは常に ``True`` 値
を返すでしょう。通常 :app:`Pyramid` アプリケーションが開始する時、それは
:term:`Configurator` に対して :term:`configuration declaration` 呼び出し
を使用して :term:`application registry` を実体化します。しかし、単体
テストからアプリケーションコードを起動する時のように、このアプリケーション
レジストリが (例えば認可ポリシーを持つ configurator の初期化によって)
作成も実体化もされなければ、 Pyramid API 関数は失敗するかデフォルトの結果
を返すことが多いです。では、このビュー関数内の :exc:`HTTPForbidden` 例外を
上げるコードの分岐をどのようにテストすればいいでしょうか。


.. The testing API provided by :app:`Pyramid` allows you to simulate various
.. application registry registrations for use under a unit testing framework
.. without needing to invoke the actual application configuration implied by its
.. ``main`` function.  For example, if you wanted to test the above ``view_fn``
.. (assuming it lived in the package named ``my.package``), you could write a
.. :class:`unittest.TestCase` that used the testing API.

:app:`Pyramid` が提供するテスト用の API を使えば、 main 関数によって
暗黙に行われる実際のアプリケーション設定を起動する必要なしに、ユニット
テストフレームワークの下で使用するための様々なアプリケーションレジストリ
の登録をシミュレートすることができます。例えば、もし上記の ``view_fn``
をテストしたければ、 (それが ``my.package`` という名のパッケージに存在
すると仮定して) テスト API を使用した :class:`unittest.TestCase` を書く
ことができます。


.. code-block:: python
   :linenos:

   import unittest
   from pyramid import testing

   class MyTest(unittest.TestCase):
       def setUp(self):
           self.config = testing.setUp()

       def tearDown(self):
           testing.tearDown()
       
       def test_view_fn_forbidden(self):
           from pyramid.httpexceptions import HTTPForbidden
           from my.package import view_fn
           self.config.testing_securitypolicy(userid='hank', 
                                              permissive=False)
           request = testing.DummyRequest()
           request.context = testing.DummyResource()
           self.assertRaises(HTTPForbidden, view_fn, request)

       def test_view_fn_allowed(self):
           from my.package import view_fn
           self.config.testing_securitypolicy(userid='hank', 
                                              permissive=True)
           request = testing.DummyRequest()
           request.context = testing.DummyResource()
           response = view_fn(request)
           self.assertEqual(response, {'greeting':'hello'})
           

.. In the above example, we create a ``MyTest`` test case that inherits from
.. :mod:`unittest.TestCase`.  If it's in our :app:`Pyramid` application, it will
.. be found when ``setup.py test`` is run.  It has two test methods.

上記の例で、 :mod:`unittest.TestCase` から継承する ``MyTest`` テストケースを
作成しています。このテストケースが :app:`Pyramid` アプリケーション内にある場合、
それは ``setup.py test`` が実行される時に見つけられます。このテストケースには
2 つのテストメソッドがあります。


.. The first test method, ``test_view_fn_forbidden`` tests the ``view_fn`` when
.. the authentication policy forbids the current user the ``edit`` permission.
.. Its third line registers a "dummy" "non-permissive" authorization policy
.. using the :meth:`~pyramid.config.Configurator.testing_securitypolicy` method,
.. which is a special helper method for unit testing.

最初のテストメソッド ``test_view_fn_forbidden`` は、認証ポリシーが現在
のユーザーに ``edit`` パーミッションを禁止している場合に ``view_fn`` を
テストします。その3行目で
:meth:`~pyramid.config.Configurator.testing_securitypolicy` メソッドを
使用して「ダミー」の「不許可」認可ポリシーを登録しています。このメソッド
は単体テストのための特別なヘルパーメソッドです。


.. We then create a :class:`pyramid.testing.DummyRequest` object which simulates
.. a WebOb request object API.  A :class:`pyramid.testing.DummyRequest` is a
.. request object that requires less setup than a "real" :app:`Pyramid` request.
.. We call the function being tested with the manufactured request.  When the
.. function is called, :func:`pyramid.security.has_permission` will call the
.. "dummy" authentication policy we've registered through
.. :meth:`~pyramid.config.Configuration.testing_securitypolicy`, which denies
.. access.  We check that the view function raises a :exc:`HTTPForbidden` error.

その後、 WebOb リクエストオブジェクト API をシミュレートする
:class:`pyramid.testing.DummyRequest` オブジェクトを作成します。
:class:`pyramid.testing.DummyRequest` は、「実際の」
:app:`Pyramid` リクエストより必要なセットアップが少ないリクエスト
オブジェクトです。そして生成されたリクエストでテスト対象の関数を
呼び出します。関数が呼ばれる時、 :func:`pyramid.security.has_permission`
は :meth:`~pyramid.config.Configuration.testing_securitypolicy` を通して
登録された「ダミー」の認証ポリシー (それはアクセスを拒否します) を
呼び出します。ビュー関数が :exc:`HTTPForbidden` エラーを上げることを
チェックします。


.. The second test method, named ``test_view_fn_allowed`` tests the alternate
.. case, where the authentication policy allows access.  Notice that we pass
.. different values to
.. :meth:`~pyramid.config.Configurator.testing_securitypolicy` to obtain this
.. result.  We assert at the end of this that the view function returns a value.

2つ目のテストメソッド ``test_view_fn_allowed`` は、別の場合 (認証ポリシー
がアクセスを許可する場合) をテストします。この結果を得るために
:meth:`~pyramid.config.Configurator.testing_securitypolicy` に異なる
値を渡していることに注目してください。最後に、ビュー関数が値を返すこと
を検証します。


.. Note that the test calls the :func:`pyramid.testing.setUp` function in its
.. ``setUp`` method and the :func:`pyramid.testing.tearDown` function in its
.. ``tearDown`` method.  We assign the result of :func:`pyramid.testing.setUp`
.. as ``config`` on the unittest class.  This is a :term:`Configurator` object
.. and all methods of the configurator can be called as necessary within
.. tests. If you use any of the :class:`~pyramid.config.Configurator` APIs during
.. testing, be sure to use this pattern in your test case's ``setUp`` and
.. ``tearDown``; these methods make sure you're using a "fresh"
.. :term:`application registry` per test run.

このテストが ``setUp`` メソッドの中で :func:`pyramid.testing.setUp` 関数を、
``tearDown`` メソッドの中で :func:`pyramid.testing.tearDown` 関数を
呼び出していることに注目してください。 :func:`pyramid.testing.setUp` の
結果を単体テストクラス上に ``config`` として代入しています。これは
:term:`Configurator` オブジェクトであり、必要ならテスト中にすべての
configurator メソッドを呼び出すことができます。テストの間に
:class:`~pyramid.config.Configurator` API のいずれかを使う場合は
必ずテストケースの ``setUp`` および ``tearDown`` の中でこのパターンを
使用してください; これらのメソッドは、テストが実行される度に必ず
「新しい」 :term:`application registry` が使用されるようにします。


.. See the :ref:`testing_module` chapter for the entire :app:`Pyramid` -specific
.. testing API.  This chapter describes APIs for registering a security policy,
.. registering resources at paths, registering event listeners, registering
.. views and view permissions, and classes representing "dummy" implementations
.. of a request and a resource.

:app:`Pyramid` 特有のテスト用 API 全体については :ref:`testing_module`
章を参照してください。本章では、セキュリティポリシーを登録したり、
リソースをパスに登録したり、イベントリスナーを登録したり、ビューと
ビューパーミッションを登録したりするための API と、リクエストやリソースの
「ダミー」の実装を表わすクラスについて記述します。


.. See also the various methods of the :term:`Configurator` documented in
.. :ref:`configuration_module` that begin with the ``testing_`` prefix.

:ref:`configuration_module` の中でドキュメント化されている、
:term:`Configurator` の ``testing_`` で始まる様々なメソッドも参照してください。


.. index::
   single: integration tests

.. Creating Integration Tests

.. _integration_tests:

結合テストの作成
--------------------------

.. In :app:`Pyramid`, a *unit test* typically relies on "mock" or "dummy"
.. implementations to give the code under test only enough context to run.

:app:`Pyramid` では、テスト下のコードを実行するのに十分なコンテキスト
のみを与えるために、*単体テスト* は典型的には「モック」や「ダミー」の
実装に依存します。


.. "Integration testing" implies another sort of testing.  In the context of a
.. :app:`Pyramid` integration test, the test logic tests the functionality of
.. some code *and* its integration with the rest of the :app:`Pyramid`
.. framework.

「結合テスト」は、別の種類のテストを意味します。 :app:`Pyramid` の結合
テストの文脈では、テストロジックはあるコードの機能性 *および*
:app:`Pyramid` フレームワークの他の部分との結合をテストします。

.. In :app:`Pyramid` applications that are plugins to Pyramid, you can create an
.. integration test by including its ``includeme`` function via
.. :meth:`pyramid.config.Configurator.include` in the test's setup code.  This
.. causes the entire :app:`Pyramid` environment to be set up and torn down as if
.. your application was running "for real".  This is a heavy-hammer way of
.. making sure that your tests have enough context to run properly, and it tests
.. your code's integration with the rest of :app:`Pyramid`.

Pyramid へのプラグインである :app:`Pyramid` アプリケーションでは、
:meth:`pyramid.config.Configurator.include` によってテストの
セットアップコードに ``includeme`` 関数を含めることにより、
結合テストを作成することができます。これによって、あたかもアプリケーションが
「本当に」作動しているかのように、 Pyramid 環境全体が構築され破棄されます。
これは、テストが適切に実行するのに十分なコンテキストを持っていることを
保証する重量級の方法です。また、それは、Pyramid の他の部分とコードの結合を
テストします。


.. Let's demonstrate this by showing an integration test for a view.  The below
.. test assumes that your application's package name is ``myapp``, and that
.. there is a ``views`` module in the app with a function with the name
.. ``my_view`` in it that returns the response 'Welcome to this application'
.. after accessing some values that require a fully set up environment.

ビューに対する結合テストを見せることでこれを実証してみましょう。下記の
テストは、アプリケーションのパッケージ名が ``myapp`` で、アプリの中に
``my_view`` という名前の関数を含む ``views`` モジュールがあり、
``my_view`` は完全な環境のセットアップを必要とするいくつかの値にアクセス
した後にレスポンス 'Welcome to this application' を返す、と仮定します。


.. code-block:: python
   :linenos:

   import unittest

   from pyramid import testing

   class ViewIntegrationTests(unittest.TestCase):
       def setUp(self):
           """ This sets up the application registry with the
           registrations your application declares in its ``includeme`` 
           function.
           """
           import myapp
           self.config = testing.setUp()
           self.config.include('myapp')

       def tearDown(self):
           """ Clear out the application registry """
           testing.tearDown()

       def test_my_view(self):
           from myapp.views import my_view
           request = testing.DummyRequest()
           result = my_view(request)
           self.assertEqual(result.status, '200 OK')
           body = result.app_iter[0]
           self.failUnless('Welcome to' in body)
           self.assertEqual(len(result.headerlist), 2)
           self.assertEqual(result.headerlist[0],
                            ('Content-Type', 'text/html; charset=UTF-8'))
           self.assertEqual(result.headerlist[1], ('Content-Length',
                                                   str(len(body))))


.. Unless you cannot avoid it, you should prefer writing unit tests that use the
.. :class:`~pyramid.config.Configurator` API to set up the right "mock"
.. registrations rather than creating an integration test.  Unit tests will run
.. faster (because they do less for each test) and the result of a unit test is
.. usually easier to make assertions about.

もし回避できるなら、結合テストの作成ではなく
:class:`~pyramid.config.Configurator` API を使って正しい「モック」登録を
セットアップする単体テストを書くべきです。単体テストはより高速に実行されます
(なぜなら各テストで行うことが少ないので)。また、単体テストの結果を検証する
ことは比較的簡単です。


.. index::
   single: functional tests

.. Creating Functional Tests

.. _functional_tests:

機能テストの作成
-------------------------

.. Functional tests test your literal application.

機能テストは文字通りのアプリケーションをテストします。


.. The below test assumes that your application's package name is ``myapp``, and
.. that there is a view that returns an HTML body when the root URL is invoked.
.. It further assumes that you've added a ``tests_require`` dependency on the
.. ``WebTest`` package within your ``setup.py`` file.  :term:`WebTest` is a
.. functional testing package written by Ian Bicking.

下記のテストは、アプリケーションのパッケージ名が ``myapp`` で、
root URL が起動された場合 HTML ボディを返すビューがある、と仮定します。
さらに、 ``setup.py`` ファイル内に ``WebTest`` パッケージに対する
``tests_require`` 依存性を追加したと仮定します。 :term:`WebTest` は
Ian Bicking によって書かれた機能テストのパッケージです。


.. code-block:: python
   :linenos:

   import unittest

   class FunctionalTests(unittest.TestCase):
       def setUp(self):
           from myapp import main
           app = main({})
           from webtest import TestApp
           self.testapp = TestApp(app)

       def test_root(self):
           res = self.testapp.get('/', status=200)
           self.failUnless('Pyramid' in res.body)


.. When this test is run, each test creates a "real" WSGI application using the
.. ``main`` function in your ``myapp.__init__`` module and uses :term:`WebTest`
.. to wrap that WSGI application.  It assigns the result to ``self.testapp``.
.. In the test named ``test_root``, we use the testapp's ``get`` method to
.. invoke the root URL.  We then assert that the returned HTML has the string
.. ``Pyramid`` in it.

このテストが実行された場合、それぞれのテストは ``myapp.__init__``
モジュール中の ``main`` 関数を使用して「実際の」 WSGI アプリケーションを
作成して、 :term:`WebTest` を使用してその WSGI アプリケーションをラップ
します。 それは ``self.testapp`` に結果を代入します。 ``test_root`` という
名前のテストでは、 root URL を起動するために testapp の ``get`` メソッドを
使用しています。その後、返された HTML の中に文字列 ``Pyramid`` が含まれて
いることを検証します。


.. See the :term:`WebTest` documentation for further information about the
.. methods available to a :class:`webtest.TestApp` instance.

:class:`webtest.TestApp` インスタンスで利用可能なメソッドに関するより詳しい
情報については、 :term:`WebTest` のドキュメントを参照してください。
