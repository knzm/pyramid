.. index::
   single: request processing
   single: request
   single: router
   single: request lifecycle

.. Request Processing

.. _router_chapter:

リクエスト処理
==================

.. Once a :app:`Pyramid` application is up and running, it is ready to accept
.. requests and return responses.  What happens from the time a :term:`WSGI`
.. request enters a :app:`Pyramid` application through to the point that
.. :app:`Pyramid` hands off a response back to WSGI for upstream processing?

一旦 :app:`Pyramid` アプリケーションが起動すると、リクエストを受け付け
てレスポンスを返す準備ができます。 :term:`WSGI` リクエストが
:app:`Pyramid` アプリケーションに入力された時から :app:`Pyramid` が上流
の処理のための WSGI にレスポンスを返すまでの間に、何が起こるのでしょうか。


.. #. A user initiates a request from his browser to the hostname and port
..    number of the WSGI server used by the :app:`Pyramid` application.

1. ユーザは、ブラウザから :app:`Pyramid` アプリケーションで使われる WSGI
   サーバのホスト名およびポート番号に対してリクエストを開始します。


.. #. The WSGI server used by the :app:`Pyramid` application passes the WSGI
..    environment to the ``__call__`` method of the :app:`Pyramid`
..    :term:`router` object.

2. :app:`Pyramid` アプリケーションによって使用される WSGI サーバは、
   :app:`Pyramid` :term:`router` オブジェクトの ``__call__`` メソッドに
   WSGI 環境変数を渡します。


.. #. A :term:`request` object is created based on the WSGI environment.

3. WSGI 環境変数に基づいて :term:`request` オブジェクトが作成されます。


.. #. The :term:`application registry` and the :term:`request` object created in
..    the last step are pushed on to the :term:`thread local` stack that
..    :app:`Pyramid` uses to allow the functions named
..    :func:`~pyramid.threadlocal.get_current_request` and
..    :func:`~pyramid.threadlocal.get_current_registry` to work.

4. 前のステップで作成された :term:`application registry` と
   :term:`request` オブジェクトが、
   :func:`~pyramid.threadlocal.get_current_request` および
   :func:`~pyramid.threadlocal.get_current_registry` という名前の関数
   が動作するように :app:`Pyramid` が使用する :term:`thread local`
   スタックに push されます。


.. #. A :class:`~pyramid.events.NewRequest` :term:`event` is sent to any
..    subscribers.

5. :class:`~pyramid.events.NewRequest` :term:`event` がすべての
   subscriber に送られます。


.. #. If any :term:`route` has been defined within application configuration,
..    the :app:`Pyramid` :term:`router` calls a :term:`URL dispatch` "route
..    mapper."  The job of the mapper is to examine the request to determine
..    whether any user-defined :term:`route` matches the current WSGI
..    environment.  The :term:`router` passes the request as an argument to the
..    mapper.

6. アプリケーション設定内に :term:`route` が定義されている場合、
   :app:`Pyramid` :term:`router` は :term:`URL dispatch` "route
   mapper" を呼び出します。 mapper の仕事は、どのユーザ定義の
   :term:`route` が現在の WSGI 環境に一致するかを判断するために
   リクエストを検査することです。 :term:`router` はリクエストを
   引数として mapper に渡します。


.. #. If any route matches, the route mapper adds attributes to the request:
..    ``matchdict`` and ``matched_route`` attributes are added to the request
..    object.  The former contains a dictionary representing the matched dynamic
..    elements of the request's ``PATH_INFO`` value, the latter contains the
..    :class:`~pyramid.interfaces.IRoute` object representing the route which
..    matched.  The root object associated with the route found is also
..    generated: if the :term:`route configuration` which matched has an
..    associated a ``factory`` argument, this factory is used to generate the
..    root object, otherwise a default :term:`root factory` is used.

7. いずれかの route が一致した場合、 route mapper はリクエストに属性を
   追加します: ``matchdict`` と ``matched_route`` 属性がリクエスト
   オブジェクトに追加されます。前者はリクエストの ``PATH_INFO`` 値の
   一致した動的要素を表わす辞書を含んでいます。後者は、一致した route を
   表わす :class:`~pyramid.interfaces.IRoute` オブジェクトを含んでいます。
   見つかった route に関連した root オブジェクトも生成されます: 一致した
   :term:`route configuration` が関連する ``factory`` 引数を持っている場合、
   この factory は root オブジェクトを生成するために使われます。
   そうでなければデフォルトの :term:`root factory` が使われます。


.. #. If a route match was *not* found, and a ``root_factory`` argument was
..    passed to the :term:`Configurator` constructor, that callable is used to
..    generate the root object.  If the ``root_factory`` argument passed to the
..    Configurator constructor was ``None``, a default root factory is used to
..    generate a root object.

8. route マッチが *見つからず* 、 ``root_factory`` 引数が
   :term:`Configurator` コンストラクタに渡された場合、その callable が
   root オブジェクトを生成するために使用されます。 Configurator
   コンストラクタに渡された ``root_factory`` 引数が ``None`` だった場合、
   デフォルトの root factory が root オブジェクトを生成するために使用されます。


.. #. The :app:`Pyramid` router calls a "traverser" function with the root
..    object and the request.  The traverser function attempts to traverse the
..    root object (using any existing ``__getitem__`` on the root object and
..    subobjects) to find a :term:`context`.  If the root object has no
..    ``__getitem__`` method, the root itself is assumed to be the context.  The
..    exact traversal algorithm is described in :ref:`traversal_chapter`. The
..    traverser function returns a dictionary, which contains a :term:`context`
..    and a :term:`view name` as well as other ancillary information.

9. :app:`Pyramid` router は、 root オブジェクトおよびリクエストを備えた
   「トラバーサー」関数を呼びます。トラバーサー関数は :term:`context`
   を見つけるために (root オブジェクトかサブオブジェクト上のいずれかの既存の
   ``__getitem__`` を使用して) root オブジェクトをトラバースしようとします。
   root オブジェクトに ``__getitem__`` メソッドがない場合、 root はそれ
   自身コンテキストであると仮定されます。正確なトラバーサルアルゴリズムは
   :ref:`traversal_chapter` で述べられています。トラバーサー機能は辞書
   を返します。それは他の補足情報と同様に :term:`context` と
   :term:`view name` も含んでいます。


.. #. The request is decorated with various names returned from the traverser
..    (such as ``context``, ``view_name``, and so forth), so they can be
..    accessed via e.g. ``request.context`` within :term:`view` code.

10. そのリクエストは、トラバーサーから返された様々な (``context`` や
    ``view_name`` などのような) 名前でデコレートされます。したがって、
    例えば :term:`view` コード内で ``request.context`` などでアクセス
    することができます、


.. #. A :class:`~pyramid.events.ContextFound` :term:`event` is sent to any
..    subscribers.

11. :class:`~pyramid.events.ContextFound` :term:`event` がすべての
    subscriber に送られます。


.. #. :app:`Pyramid` looks up a :term:`view` callable using the context, the
..    request, and the view name.  If a view callable doesn't exist for this
..    combination of objects (based on the type of the context, the type of the
..    request, and the value of the view name, and any :term:`predicate`
..    attributes applied to the view configuration), :app:`Pyramid` raises a
..    :class:`~pyramid.httpexceptions.HTTPNotFound` exception, which is meant to
..    be caught by a surrounding :term:`exception view`.

12. :app:`Pyramid` は context, request, ビュー名を使用して
    :term:`view` callable を見つけます。ビュー callable が (context の型、
    request の型、ビュー名の値、およびビュー設定に適用された任意の
    :term:`predicate` 属性に基づいて)オブジェクトのこの組み合わせに対して
    存在しない場合、 :app:`Pyramid` は
    :class:`~pyramid.httpexceptions.HTTPNotFound` 例外を上げます。
    これは、上位の :term:`exception view` によって捕捉されることを意図
    しています。


.. #. If a view callable was found, :app:`Pyramid` attempts to call it.  If an
..    :term:`authorization policy` is in use, and the view configuration is
..    protected by a :term:`permission`, :app:`Pyramid` determines whether the
..    view callable being asked for can be executed by the requesting user based
..    on credential information in the request and security information attached
..    to the context.  If the view execution is allowed, :app:`Pyramid` calls
..    the view callable to obtain a response.  If view execution is forbidden,
..    :app:`Pyramid` raises a :class:`~pyramid.httpexceptions.HTTPForbidden`
..    exception.

13. ビュー callable が見つかった場合、 :app:`Pyramid` はそれを呼び出そう
    とします。 :term:`authorization policy` が使用されており、ビュー設定が
    :term:`permission` によって保護される場合、 :app:`Pyramid` はコンテキスト
    に取り付けられたリクエストの credential 情報およびセキュリティ
    情報に基づいて、呼び出そうとしているビュー callable がリクエストした
    ユーザによって実行可能かどうかを決定します。ビュー callable が許可
    される場合、 :app:`Pyramid` はレスポンスを得るためにそのビュー callable
    を呼び出します。ビューの実行が禁止される場合、 :app:`Pyramid` は
    :class:`~pyramid.httpexceptions.HTTPForbidden` 例外を上げます。


.. #. If any exception is raised within a :term:`root factory`, by
..    :term:`traversal`, by a :term:`view callable` or by :app:`Pyramid` itself
..    (such as when it raises :class:`~pyramid.httpexceptions.HTTPNotFound` or
..    :class:`~pyramid.httpexceptions.HTTPForbidden`), the router catches the
..    exception, and attaches it to the request as the ``exception`` attribute.
..    It then attempts to find a :term:`exception view` for the exception that
..    was caught.  If it finds an exception view callable, that callable is
..    called, and is presumed to generate a response.  If an :term:`exception
..    view` that matches the exception cannot be found, the exception is
..    reraised.

14. :term:`root factory` の内で :term:`traversal` や :term:`view
    callable` あるいは :app:`Pyramid` 自身によって例外が上げられる場合
    (:class:`~pyramid.httpexceptions.HTTPNotFound` または
    :class:`~pyramid.httpexceptions.HTTPForbidden` が上がる場合のように)、
    router は例外を捕捉し、それを ``exception`` 属性として request に
    取り付けます。その後、捕捉された例外用の :term:`exception view`
    を見つけようとします。例外ビュー callable が見つかった場合、
    callable が呼ばれ、レスポンスを生成するとみなされます。
    例外と一致する :term:`exception view` を見つけることができない場合、
    例外が再送されます。


.. #. The following steps occur only when a :term:`response` could be
..    successfully generated by a normal :term:`view callable` or an
..    :term:`exception view` callable.  :app:`Pyramid` will attempt to execute
..    any :term:`response callback` functions attached via
..    :meth:`~pyramid.request.Request.add_response_callback`.  A
..    :class:`~pyramid.events.NewResponse` :term:`event` is then sent to any
..    subscribers.  The response object's ``__call__`` method is then used to
..    generate a WSGI response.  The response is sent back to the upstream WSGI
..    server.

15. 通常の :term:`view callable` あるいは :term:`exception view`
    callable によって :term:`response` を生成することに成功した場合だけ
    次のステップが生じます。 :app:`Pyramid` は、
    :meth:`~pyramid.request.Request.add_response_callback` によって
    取り付けられたすべての :term:`response callback` メソッドを実行しよ
    うとします。その後、 :class:`~pyramid.events.NewResponse`
    :term:`event` がすべての subscriber に送られます。その後、WSGI
    レスポンスを生成するために responseオブジェクトの ``__call__``
    メソッドが使用されます。レスポンスは上流の WSGI サーバに送られます。


.. #. :app:`Pyramid` will attempt to execute any :term:`finished
..    callback` functions attached via
..    :meth:`~pyramid.request.Request.add_finished_callback`.

16. :app:`Pyramid` は、
    :meth:`~pyramid.request.Request.add_finished_callback` によって取り
    付けられたすべての :term:`finished callback` 関数も実行しようとします。


.. #. The :term:`thread local` stack is popped.

17. :term:`thread local` スタックが pop されます。


.. image:: router.png

.. This is a very high-level overview that leaves out various details.  For more
.. detail about subsystems invoked by the :app:`Pyramid` router such as
.. traversal, URL dispatch, views, and event processing, see
.. :ref:`urldispatch_chapter`, :ref:`views_chapter`, and :ref:`events_chapter`.

これは様々な詳細を省略した非常に高レベルの概観です。トラバーサル、URL
ディスパッチ、ビュー、イベント処理のような :app:`Pyramid` router によって
起動されたサブシステムに関するより詳細については、
:ref:`urldispatch_chapter`, :ref:`views_chapter`, :ref:`events_chapter`
を見てください。

