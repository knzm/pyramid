.. Views

.. _views_chapter:

ビュー
======

.. One of the primary jobs of :app:`Pyramid` is to find and invoke a :term:`view
.. callable` when a :term:`request` reaches your application.  View callables
.. are bits of code which do something interesting in response to a request made
.. to your application.  They are the "meat" of any interesting web application.

:app:`Pyramid` の重要な仕事のうちの一つは、 :term:`request` が
アプリケーションに達したときに :term:`view callable` を見つけて起動する
ことです。ビュー callable は、あなたのアプリケーションになされたリクエスト
に応じて興味深いことを行うコードの断片です。それらはあらゆる面白いウェブ
アプリケーションの「肉(本質、芯)」です。


.. .. note:: 
..
..    A :app:`Pyramid` :term:`view callable` is often referred to in
..    conversational shorthand as a :term:`view`.  In this documentation,
..    however, we need to use less ambiguous terminology because there
..    are significant differences between view *configuration*, the code
..    that implements a view *callable*, and the process of view
..    *lookup*.

.. note:: 

   :app:`Pyramid` :term:`view callable` は、会話での省略表現でしばしば
   :term:`view` と呼ばれます。しかし、ビュー *設定* とビュー *callable*
   を実装するコード、およびビュー *検索* プロセスの間に著しい違いがあるので、
   このドキュメンテーションでは、それほど曖昧でない用語を使用する必要が
   あります。


.. This chapter describes how view callables should be defined. We'll have to
.. wait until a following chapter (entitled :ref:`view_config_chapter`) to find
.. out how we actually tell :app:`Pyramid` to wire up view callables to
.. particular URL patterns and other request circumstances.

本章では、ビュー callable をどのように定義するかを説明します。実際に
どのようにして特定の URL パターンおよび他のリクエスト状況に対してビュー
callable を接続するように :app:`Pyramid` に伝えるかを理解するためには、
次章 (:ref:`view_config_chapter`) まで待たなければなりません。


.. index::
   single: view callables

.. View Callables

ビュー callable
---------------

.. View callables are, at the risk of sounding obvious, callable Python
.. objects. Specifically, view callables can be functions, classes, or instances
.. that implement a ``__call__`` method (making the instance callable).

ビュー callable は、自明のことのように聞こえる危険を冒せば、 callable な
Python オブジェクトです。具体的には、ビュー callable は、関数、クラス、または
``__call__`` メソッド (これによりインスタンスが呼び出し可能になります) を
実装したインスタンスです。


.. View callables must, at a minimum, accept a single argument named
.. ``request``.  This argument represents a :app:`Pyramid` :term:`Request`
.. object.  A request object represents a :term:`WSGI` environment provided to
.. :app:`Pyramid` by the upstream WSGI server. As you might expect, the request
.. object contains everything your application needs to know about the specific
.. HTTP request being made.

ビュー callable は、最低限 ``request`` という名前の単一の引数を受け取る
必要があります。この引数は :app:`Pyramid` :term:`Request` オブジェクトを
表わします。 request オブジェクトは、上流の WSGI サーバーによって
:app:`Pyramid` に提供される :term:`WSGI` 環境変数を表わします。
予想されるように、 request オブジェクトは実行中の特定の HTTP リクエストに
関してアプリケーションが知る必要のあるすべてのものを含んでいます。


.. A view callable's ultimate responsibility is to create a :mod:`Pyramid`
.. :term:`Response` object. This can be done by creating a :term:`Response`
.. object in the view callable code and returning it directly or by raising
.. special kinds of exceptions from within the body of a view callable.

ビュー callable の最終責任は :mod:`Pyramid` :term:`Response` オブジェクト
を生成することです。これは、ビュー callable コード内で :term:`Response`
オブジェクトを生成してそれを直接返すか、あるいはビュー callable の本体
内部から特別な種類の例外を上げることによって行うことができます。


.. index::
   single: view calling convention
   single: view function


.. Defining a View Callable as a Function

.. _function_as_view:

ビュー callable を関数として定義する
--------------------------------------

.. One of the easiest way to define a view callable is to create a function that
.. accepts a single argument named ``request``, and which returns a
.. :term:`Response` object.  For example, this is a "hello world" view callable
.. implemented as a function:

ビュー callable を定義する最も簡単な方法のうちの一つは、 ``request``
という名前の単一の引数を受け取り、 :term:`Response` オブジェクトを返す
関数を作成することです。例えば、これは関数として実装された "hello world"
ビュー callable です:


.. code-block:: python
   :linenos:

   from pyramid.response import Response

   def hello_world(request):
       return Response('Hello world!')

.. index::
   single: view calling convention
   single: view class


.. Defining a View Callable as a Class

.. _class_as_view:

ビュー callable をクラスとして定義する
--------------------------------------

.. A view callable may also be represented by a Python class instead of a
.. function.  When a view callable is a class, the calling semantics are
.. slightly different than when it is a function or another non-class callable.
.. When a view callable is a class, the class' ``__init__`` method is called with a
.. ``request`` parameter.  As a result, an instance of the class is created.
.. Subsequently, that instance's ``__call__`` method is invoked with no
.. parameters.  Views defined as classes must have the following traits:

ビュー callable は関数の代わりに Python クラスによっても表すことができます。
ビュー callable がクラスである場合、それが関数または別の非クラスの callable
である場合とは呼び出しのセマンティクスがやや異なります。ビュー callable
がクラスである場合、クラスの ``__init__`` メソッドが ``request`` パラメータ
を伴って呼ばれます。その結果、クラスのインスタンスが生成されます。続いて、
そのインスタンスの ``__call__`` メソッドがパラメータなしで起動されます。
クラスとして定義されたビューには次のような特性がなければなりません:


.. - an ``__init__`` method that accepts a ``request`` argument.

- ``request`` 引数を受け取る ``__init__`` メソッド。


.. - a ``__call__`` (or other) method that accepts no parameters and which
..   returns a response.

- パラメータを受け取らず、レスポンスを返す ``__call__`` (あるいは他の)
  メソッド。


.. For example:

例えば:


.. code-block:: python
   :linenos:

   from pyramid.response import Response

   class MyView(object):
       def __init__(self, request):
           self.request = request

       def __call__(self):
           return Response('hello')


.. The request object passed to ``__init__`` is the same type of request object
.. described in :ref:`function_as_view`.

``__init__`` に渡された request オブジェクトは :ref:`function_as_view`
で述べたのと同じ型の request オブジェクトです。


.. If you'd like to use a different attribute than ``__call__`` to represent the
.. method expected to return a response, you can use an ``attr`` value as part 
.. of the configuration for the view.  See :ref:`view_configuration_parameters`.
.. The same view callable class can be used in different view configuration 
.. statements with different ``attr`` values, each pointing at a different 
.. method of the class if you'd like the class to represent a collection of 
.. related view callables.

レスポンスを返すことが期待されるメソッドを表わすために ``__call__`` と
は別の属性を使用したければ、ビュー設定の一部として ``attr`` 値を使用する
ことができます。 :ref:`view_configuration_parameters` を参照してください。
クラスが関連するビュー callable のコレクションを表わすようにしたければ、
各々クラスの異なるメソッドを指す異なる `attr` 値を用いて、同じビュー
callable クラスを異なるビュー設定の中で使用することができます。


.. index::
   single: view response
   single: response


.. View Callable Responses

.. _the_response:

ビュー callable レスポンス
--------------------------

.. A view callable may return an object that implements the :app:`Pyramid`
.. :term:`Response` interface.  The easiest way to return something that
.. implements the :term:`Response` interface is to return a
.. :class:`pyramid.response.Response` object instance directly.  For example:

ビュー callable は、 :app:`Pyramid` :term:`Response` インタフェースを
実装するオブジェクトを返すことができます。 :term:`Response` インタフェース
を実装するものを返す最も簡単な方法は、
:class:`pyramid.response.Response` オブジェクトのインスタンスを直接
返すことです。例えば:


.. code-block:: python
   :linenos:

   from pyramid.response import Response

   def view(request):
       return Response('OK')


.. :app:`Pyramid` provides a range of different "exception" classes which
.. inherit from :class:`pyramid.response.Response`.  For example, an instance of
.. the class :class:`pyramid.httpexceptions.HTTPFound` is also a valid response
.. object because it inherits from :class:`~pyramid.response.Response`.  For
.. examples, see :ref:`http_exceptions` and :ref:`http_redirect`.

:app:`Pyramid` は、 :class:`pyramid.response.Response` を継承する一連の
様々な「例外」クラスを提供しています。例えば、クラス
:class:`pyramid.httpexceptions.HTTPFound` のインスタンスは
:class:`~pyramid.response.Response` を継承するので、これも有効な
response オブジェクトです。例については、 :ref:`http_exceptions` と
:ref:`http_redirect` を参照してください。


.. note::

   .. You can also return objects from view callables that aren't instances of
   .. :class:`pyramid.response.Response` in various circumstances.  This can be
   .. helpful when writing tests and when attempting to share code between view
   .. callables.  See :ref:`renderers_chapter` for the common way to allow for
   .. this.  A much less common way to allow for view callables to return
   .. non-Response objects is documented in :ref:`using_iresponse`.

   様々な状況で :class:`pyramid.response.Response` のインスタンスでない
   オブジェクトをビュー callable から返すこともできます。
   これはテストを書くときや複数のビュー callable の間でコードを共有しようと
   するときに有用なことがあります。これを可能にする通常の方法については
   :ref:`renderers_chapter` を参照してください。それよりもまれな、
   ビュー callable が Response オブジェクト以外を返すことを可能にする方法は
   :ref:`using_iresponse` の中で文書化されます。


.. index::
   single: view exceptions


.. Using Special Exceptions In View Callables

.. _special_exceptions_in_callables:

ビュー callable の中で特別な例外を使用する
------------------------------------------

.. Usually when a Python exception is raised within a view callable,
.. :app:`Pyramid` allows the exception to propagate all the way out to the
.. :term:`WSGI` server which invoked the application.  It is usually caught and
.. logged there.

通常、ビュー callable 内で Python 例外が上がった場合、 :app:`Pyramid` は
アプリケーションを起動した :term:`WSGI` サーバまで例外が伝搬することを
許します。例外は通常そこで捕捉されてログに記録されます。


.. However, for convenience, a special set of exceptions exists.  When one of
.. these exceptions is raised within a view callable, it will always cause
.. :app:`Pyramid` to generate a response.  These are known as :term:`HTTP
.. exception` objects.

しかし、利便性のために特別な例外のセットが存在しています。これらの例外が
ビュー callable 内で上がった場合、 :app:`Pyramid` によって常にレスポンスが
生成されます。これらは :term:`HTTP exception` オブジェクトとして知られています。


.. index::
   single: HTTP exceptions


.. HTTP Exceptions

.. _http_exceptions:

HTTP 例外
~~~~~~~~~~~~~~~

.. All classes documented in the :mod:`pyramid.httpexceptions` module documented
.. as inheriting from the :class:`pryamid.httpexceptions.HTTPException` are
.. :term:`http exception` objects.  Instances of an HTTP exception object may
.. either be *returned* or *raised* from within view code.  In either case
.. (return or raise) the instance will be used as as the view's response.

:mod:`pyramid.httpexceptions` モジュールの中で
:class:`pryamid.httpexceptions.HTTPException` から継承すると文書化
されたすべてのクラスは :term:`http exception` オブジェクトです。
HTTP 例外オブジェクトのインスタンスは、ビューコードの内部から
*戻り値として返される* か、または *例外として投げられ* ます。
いずれの場合も、そのインスタンスはビューからのレスポンスとして
使用されます。


.. For example, the :class:`pyramid.httpexceptions.HTTPUnauthorized` exception
.. can be raised.  This will cause a response to be generated with a ``401
.. Unauthorized`` status:

例えば、 :class:`pyramid.httpexceptions.HTTPUnauthorized` 例外を投げる
ことができます。これは ``401 Unauthorized`` ステータスでレスポンスを生成
するでしょう:


.. code-block:: python
   :linenos:

   from pyramid.httpexceptions import HTTPUnauthorized

   def aview(request):
       raise HTTPUnauthorized()


.. An HTTP exception, instead of being raised, can alternately be *returned*
.. (HTTP exceptions are also valid response objects):

HTTP 例外は raise する代わりに *返す* こともできます
(HTTP 例外は有効な response オブジェクトでもあります):


.. code-block:: python
   :linenos:

   from pyramid.httpexceptions import HTTPUnauthorized

   def aview(request):
       return HTTPUnauthorized()


.. A shortcut for creating an HTTP exception is the
.. :func:`pyramid.httpexceptions.exception_response` function.  This function
.. accepts an HTTP status code and returns the corresponding HTTP exception.
.. For example, instead of importing and constructing a
.. :class:`~pyramid.httpexceptions.HTTPUnauthorized` response object, you can
.. use the :func:`~pyramid.httpexceptions.exception_response` function to
.. construct and return the same object.

HTTP 例外を生成するための近道は
:func:`pyramid.httpexceptions.exception_response` 関数です。この関数は、
HTTP ステータスコードを受け取り対応する HTTP 例外を返します。例えば、
:class:`~pyramid.httpexceptions.HTTPUnauthorized` をインポートして
response オブジェクトを構築する代わりに、同じオブジェクトを構築して返す
ために :func:`~pyramid.httpexceptions.exception_response` 関数を使用する
ことができます。


.. code-block:: python
   :linenos:

   from pyramid.httpexceptions import exception_response

   def aview(request):
       raise exception_response(401)


.. This is the case because ``401`` is the HTTP status code for "HTTP
.. Unauthorized".  Therefore, ``raise exception_response(401)`` is functionally
.. equivalent to ``raise HTTPUnauthorized()``.  Documentation which maps each
.. HTTP response code to its purpose and its associated HTTP exception object is
.. provided within :mod:`pyramid.httpexceptions`.

これは 401 が "HTTP Unauthorized" のための HTTP ステータスコードであるためです。
したがって、 ``raise exception_response(401)`` は
``raise HTTPUnauthorized()`` と機能的に等価です。
:mod:`pyramid.httpexceptions` の中で、各 HTTP レスポンスコードから
その目的および関連する HTTP 例外オブジェクトにマッピングする
ドキュメンテーションが提供されます。


.. .. note:: The :func:`~pyramid.httpexceptions.exception_response` function is
..    new as of Pyramid 1.1.

.. note::

   :func:`~pyramid.httpexceptions.exception_response` 関数は
   Pyramid 1.1 で新たに追加されました。


.. How Pyramid Uses HTTP Exceptions

Pyramid は HTTP 例外をどのように使用するか
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. HTTP exceptions are meant to be used directly by application
.. developers.  However, Pyramid itself will raise two HTTP exceptions at
.. various points during normal operations:
.. :exc:`pyramid.httpexceptions.HTTPNotFound` and
.. :exc:`pyramid.httpexceptions.HTTPForbidden`.  Pyramid will raise the
.. :exc:`~pyramid.httpexceptions.HTTPNotFound` exception are raised when it
.. cannot find a view to service a request.  Pyramid will raise the
.. :exc:`~pyramid.httpexceptions.Forbidden` exception or when authorization was
.. forbidden by a security policy.

HTTP 例外はアプリケーション開発者が直接使うためのものです。
しかし、 Pyramid は自身も通常動作の間に様々なポイントで
2 つの HTTP 例外を上げます: :exc:`pyramid.httpexceptions.HTTPNotFound` と
:exc:`pyramid.httpexceptions.HTTPForbidden` です。
リクエストをサービスするビューが見つからない場合、 Pyramid は
:exc:`~pyramid.httpexceptions.HTTPNotFound` 例外を上げます。
認可がセキュリティポリシーによって禁止された場合、 Pyramid は
:exc:`~pyramid.httpexceptions.Forbidden` 例外を上げます。


.. If :exc:`~pyramid.httpexceptions.HTTPNotFound` is raised by Pyramid itself or
.. within view code, the result of the :term:`Not Found View` will be returned
.. to the user agent which performed the request.

:exc:`~pyramid.httpexceptions.HTTPNotFound` が Pyramid 自体によって、
またはビューコード内で上げられる場合、 :term:`Not Found View` の結果が
リクエストを行なったユーザエージェントに返されます。


.. If :exc:`~pyramid.httpexceptions.HTTPForbidden` is raised by Pyramid itself
.. within view code, the result of the :term:`Forbidden View` will be returned
.. to the user agent which performed the request.

:exc:`~pyramid.httpexceptions.HTTPForbidden` が Pyramid 自体によって、
またはビューコード内で上げられる場合、 :term:`Forbidden View` の結果が
リクエストを行なったユーザエージェントに返されます。


.. index::
   single: exception views


.. Custom Exception Views

.. _exception_views:

カスタム例外ビュー
----------------------

.. The machinery which allows HTTP exceptions to be raised and caught by
.. specialized views as described in :ref:`special_exceptions_in_callables` can
.. also be used by application developers to convert arbitrary exceptions to
.. responses.

:ref:`special_exceptions_in_callables` に述べられているように、 HTTP
例外が投げられて専用のビューによって補足されることを可能にする機構を、
アプリケーション開発者もレスポンスの任意の例外を変換するために使用する
ことができます。


.. To register a view that should be called whenever a particular exception is
.. raised from with :app:`Pyramid` view code, use the exception class or one of
.. its superclasses as the ``context`` of a view configuration which points at a
.. view callable you'd like to generate a response.

特定の例外が :app:`Pyramid` ビューコードから上がった場合に常に呼ばれる
ビューを登録するためには、レスポンスを生成したいビュー callable を指す
ビュー設定の ``context`` として例外クラスあるいはその親クラスのうちの
一つを使用してください。


.. For example, given the following exception class in a module named
.. ``helloworld.exceptions``:

例えば、 ``helloworld.exceptions`` という名前のモジュールに次のような
例外クラスがあるとします:


.. code-block:: python
   :linenos:

   class ValidationFailure(Exception):
       def __init__(self, msg):
           self.msg = msg


.. You can wire a view callable to be called whenever any of your *other* code
.. raises a ``helloworld.exceptions.ValidationFailure`` exception:

どこかにある *他の* コードが ``helloworld.exceptions.ValidationFailure``
例外を上げた場合には常に呼ばれるようにビュー callable を設定することができます:


.. code-block:: python
   :linenos:

   from pyramid.view import view_config
   from helloworld.exceptions import ValidationFailure

   @view_config(context=ValidationFailure)
   def failed_validation(exc, request):
       response =  Response('Failed validation: %s' % exc.msg)
       response.status_int = 500
       return response


.. Assuming that a :term:`scan` was run to pick up this view registration, this
.. view callable will be invoked whenever a
.. ``helloworld.exceptions.ValidationFailure`` is raised by your application's
.. view code.  The same exception raised by a custom root factory, a custom
.. traverser, or a custom view or route predicate is also caught and hooked.

このビュー登録を拾い上げるために :term:`scan` が実行されたとすると、
このビュー callable はアプリケーションのビューコードによって
``helloworld.exceptions.ValidationFailure`` が上げられる場合は常に起動
されることになります。カスタム root ファクトリ、カスタムトラバーサ、
あるいはカスタムビュー、またはルート述語によって上げられた同じ例外も
捕捉されフックされます。


.. Other normal view predicates can also be used in combination with an
.. exception view registration:

他の通常のビュー述語も例外ビュー登録と組み合わせて使用できます:


.. code-block:: python
   :linenos:

   from pyramid.view import view_config
   from helloworld.exceptions import ValidationFailure

   @view_config(context=ValidationFailure, route_name='home')
   def failed_validation(exc, request):
       response =  Response('Failed validation: %s' % exc.msg)
       response.status_int = 500
       return response


.. The above exception view names the ``route_name`` of ``home``, meaning that
.. it will only be called when the route matched has a name of ``home``.  You
.. can therefore have more than one exception view for any given exception in
.. the system: the "most specific" one will be called when the set of request
.. circumstances match the view registration.

上記の例外ビューは ``route_name`` に ``home`` と指定しています。これは
ルート一致が ``home`` という名前を持っている時にだけそれが呼ばれること
を意味しています。そのため、ある例外に対する例外ビューをシステムの中に
複数持つことが可能です: リクエスト状況のセットがビュー登録と一致する時は
「最も特殊な」ものが呼ばれます。


.. The only view predicate that cannot be used successfully when creating
.. an exception view configuration is ``name``.  The name used to look up
.. an exception view is always the empty string.  Views registered as
.. exception views which have a name will be ignored.

例外ビュー設定を行う場合、正常に使用できない唯一のビュー述語は
``name`` です。例外ビューを検索するために使用される名前は常に空の文字列
です。例外ビューとして登録された名前を持つビューは無視されます。


.. note::

  .. Normal (i.e., non-exception) views registered against a context resource
  .. type which inherits from :exc:`Exception` will work normally.  When an
  .. exception view configuration is processed, *two* views are registered.  One
  .. as a "normal" view, the other as an "exception" view.  This means that you
  .. can use an exception as ``context`` for a normal view.

  :exc:`Exception` から継承したコンテキストリソース型に対して登録された
  通常の (つまり例外ではない) ビューは、正常に動作します。例外ビュー設定が
  処理される場合、 *2つの* ビューが登録されます。一方は「通常の」ビュー
  として、他方は「例外」ビューとして。これは、 ``context`` として例外を
  通常のビューに使用できることを意味します。


.. Exception views can be configured with any view registration mechanism:
.. ``@view_config`` decorator or imperative ``add_view`` styles.

例外ビューは任意のビュー登録メカニズムで構成することができます:
``@view_config`` デコレータまたは命令的な ``add_view`` スタイル。


.. index::
   single: view http redirect
   single: http redirect (from a view)


.. Using a View Callable to Do an HTTP Redirect

.. _http_redirect:

HTTP リダイレクトを行うためにビュー callable を使う
---------------------------------------------------

.. You can issue an HTTP redirect by using the
.. :class:`pyramid.httpexceptions.HTTPFound` class.  Raising or returning an
.. instance of this class will cause the client to receive a "302 Found"
.. response.

:class:`pyramid.httpexceptions.HTTPFound` クラスを使用して HTTP
リダイレクトを発行することができます。このクラスのインスタンスを
例外として上げるか戻り値として返せば、クライアントは "302 Found"
レスポンスを受け取るでしょう。


.. To do so, you can *return* a :class:`pyramid.httpexceptions.HTTPFound`
.. instance.

これを行うために、 :class:`pyramid.httpexceptions.HTTPFound` インスタンス
を *返す* ことができます。


.. code-block:: python
   :linenos:

   from pyramid.httpexceptions import HTTPFound

   def myview(request):
       return HTTPFound(location='http://example.com')


.. Alternately, you can *raise* an HTTPFound exception instead of returning one.

または、それを返す代わりに HTTPFound 例外を上げることができます。


.. code-block:: python
   :linenos:

   from pyramid.httpexceptions import HTTPFound

   def myview(request):
       raise HTTPFound(location='http://example.com')


.. When the instance is raised, it is caught by the default :term:`exception
.. response` handler and turned into a response.

インスタンスが例外として上げられる場合、それはデフォルト
:term:`exception response` ハンドラによって捕捉され、レスポンスへと変換
されます。


.. index::
   single: unicode, views, and forms
   single: forms, views, and unicode
   single: views, forms, and unicode

.. Handling Form Submissions in View Callables (Unicode and Character Set Issues)

ビュー callable の中でフォーム送信を扱う (Unicodeと文字集合の問題)
------------------------------------------------------------------------------

.. Most web applications need to accept form submissions from web browsers and
.. various other clients.  In :app:`Pyramid`, form submission handling logic is
.. always part of a :term:`view`.  For a general overview of how to handle form
.. submission data using the :term:`WebOb` API, see :ref:`webob_chapter` and
.. `"Query and POST variables" within the WebOb documentation
.. <http://docs.webob.org/en/latest/reference.html#query-post-variables>`_.
.. :app:`Pyramid` defers to WebOb for its request and response implementations,
.. and handling form submission data is a property of the request
.. implementation.  Understanding WebOb's request API is the key to
.. understanding how to process form submission data.

ほとんどのウェブアプリケーションは、ウェブブラウザおよび様々な他の
クライアントからフォーム送信を受け取る必要があります。 :app:`Pyramid` では、
フォーム送信を扱うロジックは常に :term:`view` の一部です。
:term:`WebOb` APIを使用してフォーム送信データを処理する方法の一般的な
概要については、 :ref:`webob_chapter` と `WebObドキュメンテーション内の
"Query and POST variables"
<http://docs.webob.org/en/latest/reference.html#query-post-variables>`_.
を見てください。 :app:`Pyramid` は、リクエストとレスポンスの実装については
WebOb に任せています。また、フォーム送信データの取り扱いはリクエスト実装の
特性です。 WebOb のリクエスト API を理解することは、フォーム送信データを
処理する方法を理解するための鍵です。


.. There are some defaults that you need to be aware of when trying to handle
.. form submission data in a :app:`Pyramid` view.  Having high-order (i.e.,
.. non-ASCII) characters in data contained within form submissions is
.. exceedingly common, and the UTF-8 encoding is the most common encoding used
.. on the web for character data. Since Unicode values are much saner than
.. working with and storing bytestrings, :app:`Pyramid` configures the
.. :term:`WebOb` request machinery to attempt to decode form submission values
.. into Unicode from UTF-8 implicitly.  This implicit decoding happens when view
.. code obtains form field values via the ``request.params``, ``request.GET``,
.. or ``request.POST`` APIs (see :ref:`request_module` for details about these
.. APIs).

:app:`Pyramid` ビュー内でフォーム送信データを処理しようとする場合、
意識する必要のあるいくつかのデフォルトがあります。フォーム送信データに
上位ビットが立った (つまり非 ASCII) 文字が含まれていることは、
非常に一般的です。また、 UTF-8 符号化はウェブ上で文字データに使用される
最も一般的な符号化です。バイト文字列を扱ったり格納したりするよりも
Unicode 値の方がはるかに健全なので、 :app:`Pyramid` はフォーム送信値を
可能なら UTF-8 から Unicode へ暗黙的にデコードするように :term:`WebOb`
リクエスト機能を構成します。ビューコードが ``request.params``,
``request.GET`` あるいは ``request.POST`` API によってフォームフィールド
値を得る場合、暗黙のデコードが起こります (これらの API に関する詳細に
関しては :ref:`request_module` を参照)


.. .. note::

..    Many people find the difference between Unicode and UTF-8 confusing.
..    Unicode is a standard for representing text that supports most of the
..    world's writing systems. However, there are many ways that Unicode data
..    can be encoded into bytes for transit and storage. UTF-8 is a specific
..    encoding for Unicode, that is backwards-compatible with ASCII. This makes
..    UTF-8 very convenient for encoding data where a large subset of that data
..    is ASCII characters, which is largely true on the web. UTF-8 is also the
..    standard character encoding for URLs.

.. note::

   多くの人々が Unicode と UTF-8 の間の違いに混乱を見い出します。
   Unicode は、世界の書記体系のうちのほとんどをサポートするテキストを表わす
   ための規格です。しかし、転送と記録のために Unicode データをバイトへ
   エンコードするための多くの方法があります。 UTF-8 は Unicode のための
   特定の符号化で、 ASCII と後方互換性を持ちます。このため、データの
   大部分が ASCII 文字であるような場合に UTF-8 はデータをエンコードする
   のに非常に便利です。それはウェブ上では大部分正しいです。 UTF-8 は
   URL のための標準文字符号化でもあります。


.. As an example, let's assume that the following form page is served up to a
.. browser client, and its ``action`` points at some :app:`Pyramid` view code:

例として、次のようなフォームページがブラウザクライアントに表示され、
その ``action`` が何らかの :app:`Pyramid` ビューコードを指していると
仮定しましょう:


.. code-block:: xml
   :linenos:

   <html xmlns="http://www.w3.org/1999/xhtml">
     <head>
       <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
     </head>
     <form method="POST" action="myview">
       <div>
         <input type="text" name="firstname"/>
       </div> 
       <div>
         <input type="text" name="lastname"/>
       </div>
       <input type="submit" value="Submit"/>
     </form>
   </html>


.. The ``myview`` view code in the :app:`Pyramid` application *must* expect that
.. the values returned by ``request.params`` will be of type ``unicode``, as
.. opposed to type ``str``. The following will work to accept a form post from
.. the above form:

:app:`Pyramid` アプリケーション内の ``myview`` ビューコードは、
``request.params`` によって返された値が ``str`` 型ではなく ``unicode``
型になると想定 *しなければなりません* 。上記のフォームからフォームポスト
を受け取るのに下記のコードは動くでしょう:


.. code-block:: python
   :linenos:

   def myview(request):
       firstname = request.params['firstname']
       lastname = request.params['lastname']


.. But the following ``myview`` view code *may not* work, as it tries to decode
.. already-decoded (``unicode``) values obtained from ``request.params``:

しかし、次の ``myview`` ビューコードは動かないかもしれません。
``request.params`` から取得したデコード済みの (``unicode``) 値を
デコードしようとしているからです:


.. code-block:: python
   :linenos:

   def myview(request):
       # the .decode('utf-8') will break below if there are any high-order
       # characters in the firstname or lastname
       firstname = request.params['firstname'].decode('utf-8')
       lastname = request.params['lastname'].decode('utf-8')


.. For implicit decoding to work reliably, you should ensure that every form you
.. render that posts to a :app:`Pyramid` view explicitly defines a charset
.. encoding of UTF-8. This can be done via a response that has a
.. ``;charset=UTF-8`` in its ``Content-Type`` header; or, as in the form above,
.. with a ``meta http-equiv`` tag that implies that the charset is UTF-8 within
.. the HTML ``head`` of the page containing the form.  This must be done
.. explicitly because all known browser clients assume that they should encode
.. form data in the same character set implied by ``Content-Type`` value of the
.. response containing the form when subsequently submitting that form. There is
.. no other generally accepted way to tell browser clients which charset to use
.. to encode form data.  If you do not specify an encoding explicitly, the
.. browser client will choose to encode form data in its default character set
.. before submitting it, which may not be UTF-8 as the server expects.  If a
.. request containing form data encoded in a non-UTF8 charset is handled by your
.. view code, eventually the request code accessed within your view will throw
.. an error when it can't decode some high-order character encoded in another
.. character set within form data, e.g., when ``request.params['somename']`` is
.. accessed.

暗黙のデコードが確実 (reliably) に動作するため、 :app:`Pyramid` ビューに
ポストするすべてのフォームをレンダリングする際は必ず明示的に charset
エンコーディングを UTF-8 と定義するようにします。これは、レスポンスの
``Content-Type`` ヘッダの中で ``;charset=UTF-8`` を指定することによって、
あるいは上記のフォームのようにフォームを含むページの HTML ``head`` で
charset が UTF-8 であることを示す ``meta http-equiv`` タグによって
行うことができます。既知のすべてのブラウザクライアントは、フォームを
送信する際にはそのフォームを含むレスポンスの ``Content-Type`` 値によって
推測されたのと同じ文字集合を使ってフォームデータをエンコードすべきで
あると仮定するので、これは明示的に行われなければなりません。フォームデータ
をエンコードするためにどの charset を使用すべきかをブラウザクライアント
に伝える一般的に受け入れられた方法はこれ以外にありません。エンコードを
明示的に指定しなければ、ブラウザクライアントはフォームデータを送信する前に
デフォルト文字セットにエンコードすることを選択します。それはサーバーが
期待する UTF-8 ではない可能性があります。非 UTF8 charset でエンコードされた
フォームデータを含むリクエストがビューコードによって扱われるなら、いずれ
他の文字集合でエンコードされたフォームデータ内の 8 bit 文字をデコード
できずにビュー内で実行されたリクエストコードで
(例えば ``request.params['somename']`` がアクセスされた場合に)
エラーが発生するでしょう。


.. If you are using the :class:`~pyramid.response.Response` class to generate a
.. response, or if you use the ``render_template_*`` templating APIs, the UTF-8
.. charset is set automatically as the default via the ``Content-Type`` header.
.. If you return a ``Content-Type`` header without an explicit charset, a
.. request will add a ``;charset=utf-8`` trailer to the ``Content-Type`` header
.. value for you, for response content types that are textual
.. (e.g. ``text/html``, ``application/xml``, etc) as it is rendered.  If you are
.. using your own response object, you will need to ensure you do this yourself.

レスポンスを生成するために :class:`~pyramid.response.Response` クラスを
使用しているか、あるいは ``render_template_`` テンプレート API を使用して
いれば、 ``Content-Type`` ヘッダーに UTF-8 charset がデフォルトとして
自動的にセットされます。明示的な charset のない ``Content-Type`` ヘッダ
を返した場合、テキスト型のレスポンスコンテントタイプ (例えば
``text/html`` や ``application/xml`` など) に対しては、あなたの代わりに
リクエスト (訳注: レスポンスの間違い?) がレンダリング時に ``Content-Type``
ヘッダー値に ``;charset=utf-8`` トレーラーを加えます。独自のレスポンス
オブジェクトを使用していれば、これを自分自身で確実に行う必要があるでしょう。


.. .. note:: Only the *values* of request params obtained via
..    ``request.params``, ``request.GET`` or ``request.POST`` are decoded
..    to Unicode objects implicitly in the :app:`Pyramid` default
..    configuration.  The keys are still (byte) strings.

.. note:: ``request.params``, ``request.GET``, ``request.POST`` によって
   得られるリクエスト params の *値* だけが :app:`Pyramid` デフォルト設定の
   もとでユニコードオブジェクトに暗黙にデコードされます。キーは、
   依然として(バイト)文字列です。



.. index::
   single: view calling convention


.. Alternate View Callable Argument/Calling Conventions

.. _request_and_context_view_definitions:

ビュー callable の引数/呼び出し規約を変更する
----------------------------------------------------

.. Usually, view callables are defined to accept only a single argument:
.. ``request``.  However, view callables may alternately be defined as classes,
.. functions, or any callable that accept *two* positional arguments: a
.. :term:`context` resource as the first argument and a :term:`request` as the
.. second argument.

通常、ビュー callable は単一の引数 ``request`` だけを受け取ると定義されます。
しかし、その代わりにビュー callable はクラス、関数あるいは *2 つ* の位置引数
を受け取る何らかの callableとして定義されることも可能です: 1つ目の引数と
して :term:`context` リソース、 2 つ目の引数として :term:`request` です。


.. The :term:`context` and :term:`request` arguments passed to a view function
.. defined in this style can be defined as follows:

このスタイルで定義されたビュー関数に渡される :term:`context` と
:term:`request` の各引数は、以下のように定義することができます:


context

  .. The :term:`resource` object found via tree :term:`traversal` or :term:`URL
  .. dispatch`.

  ツリー :term:`traversal` あるいは :term:`URL dispatch` によって見つかった
  :term:`resource` オブジェクト。


request

  .. A :app:`Pyramid` Request object representing the current WSGI request.

  現在の WSGI リクエストを表わす :app:`Pyramid` リクエストオブジェクト。


.. The following types work as view callables in this style:

次のタイプがこのスタイルのビュー callable として動作します:


.. #. Functions that accept two arguments: ``context``, and ``request``,
..    e.g.:

1. 2つの引数 ``context``, ``request`` を受け取る関数。例えば:


   .. code-block:: python
	  :linenos:

	  from pyramid.response import Response

	  def view(context, request):
		  return Response('OK')


.. #. Classes that have an ``__init__`` method that accepts ``context,
..    request`` and a ``__call__`` method which accepts no arguments, e.g.:

2. ``context, request`` を受け取る ``__init__`` メソッドと、
   引数を受け取らない ``__call__`` メソッドを持つクラス。例えば:


   .. code-block:: python
	  :linenos:

	  from pyramid.response import Response

	  class view(object):
		  def __init__(self, context, request):
			  self.context = context
			  self.request = request

		  def __call__(self):
			  return Response('OK')


.. #. Arbitrary callables that have a ``__call__`` method that accepts
..    ``context, request``, e.g.:

3. ``context,request`` を受け取る ``__call__`` メソッドを持つ
   任意の callable 。例えば:


   .. code-block:: python
	  :linenos:

	  from pyramid.response import Response

	  class View(object):
		  def __call__(self, context, request):
			  return Response('OK')
	  view = View() # this is the view callable


.. This style of calling convention is most useful for :term:`traversal` based
.. applications, where the context object is frequently used within the view
.. callable code itself.

このスタイルの呼び出し規約は、コンテキストオブジェクトがビュー callable
のコード自身によって頻繁に使用される :term:`traversal` に基づく
アプリケーションで最も有用です。


.. No matter which view calling convention is used, the view code always has
.. access to the context via ``request.context``.

どのビュー呼び出し規約が使われていても、ビューコードは常に
``request.context`` によってコンテキストにアクセスする必要があります。


.. index::
   single: Pylons-style controller dispatch

.. Pylons-1.0-Style "Controller" Dispatch

Pylons 1.0 スタイルの "コントローラ" ディスパッチ
--------------------------------------------------

.. A package named :term:`pyramid_handlers` (available from PyPI) provides an
.. analogue of :term:`Pylons` -style "controllers", which are a special kind of
.. view class which provides more automation when your application uses
.. :term:`URL dispatch` solely.

:term:`pyramid_handlers` という名のパッケージ (PyPI から利用可能) は
:term:`Pylons` スタイルの「コントローラ」の類似品を提供します。
それは、アプリケーションが :term:`URL dispatch` を単独で使用する場合に
より多くの自動化を提供する特別な種類のビュークラスです。

