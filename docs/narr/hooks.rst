.. Using Hooks

.. _hooks_chapter:

フックの使用
============

.. "Hooks" can be used to influence the behavior of the :app:`Pyramid` framework
.. in various ways.

:app:`Pyramid` フレームワークの振る舞いに影響を及ぼすために、様々な方法で
「フック」を使用することができます。


.. index::
   single: not found view

.. Changing the Not Found View

.. _changing_the_notfound_view:

Not Found ビューの変更
---------------------------

.. When :app:`Pyramid` can't map a URL to view code, it invokes a :term:`not
.. found view`, which is a :term:`view callable`. A default notfound view
.. exists.  The default not found view can be overridden through application
.. configuration.

:app:`Pyramid` は URL をビューコードにマッピングすることができない場合
:term:`not found view` を起動します。それは :term:`view callable` です。
デフォルトの not found ビューが存在します。デフォルトの not found ビュー
は、アプリケーション設定によってオーバーライドすることができます。


.. If your application uses :term:`imperative configuration`, you can replace
.. the Not Found view by using the
.. :meth:`pyramid.config.Configurator.add_notfound_view` method:

アプリケーションが :term:`imperative configuration` を使用している場合、
:meth:`pyramid.config.Configurator.add_notfound_view` メソッドを使って
Not Found ビューを置き換えることができます:


.. code-block:: python
   :linenos:

   from helloworld.views import notfound
   config.add_notfound_view(notfound)


.. Replace ``helloworld.views.notfound`` with a reference to the :term:`view
.. callable` you want to use to represent the Not Found view.  The :term:`not
.. found view` callable is a view callable like any other.

``helloworld.views.notfound`` を、 Not Found ビューとして使用したい
:term:`view callable` への参照に置き換えてください。
:term:`not found view` callable は、他と同様のビュー callable です。


.. If your application instead uses :class:`pyramid.view.view_config` decorators
.. and a :term:`scan`, you can replace the Not Found view by using the
.. :class:`pyramid.view.notfound_view_config` decorator:

一方、アプリケーションが :class:`pyramid.view.view_config` デコレータと
:term:`scan` を使用している場合、
:class:`pyramid.view.notfound_view_config` デコレータを使うことで
Not Found ビューを交換することができます:


.. code-block:: python
   :linenos:

   from pyramid.view import notfound_view_config

   @notfound_view_config()
   def notfound(request):
       return Response('Not Found, dude', status='404 Not Found')

   def main(globals, **settings):
      config = Configurator()
      config.scan()


.. This does exactly what the imperative example above showed.

これは、上記の命令型 (imperative) の例で示したのとまったく同じことを
行います。


.. Your application can define *multiple* not found views if necessary.  Both
.. :meth:`pyramid.config.Configurator.add_notfound_view` and
.. :class:`pyramid.view.notfound_view_config` take most of the same arguments as
.. :class:`pyramid.config.Configurator.add_view` and
.. :class:`pyramid.view.view_config`, respectively.  This means that not found
.. views can carry predicates limiting their applicability.  For example:

アプリケーションは、必要なら *複数の* not found ビューを定義することができます。
:meth:`pyramid.config.Configurator.add_notfound_view` と
:class:`pyramid.view.notfound_view_config` の両方は、それぞれ
:class:`pyramid.config.Configurator.add_view` と
:class:`pyramid.view.view_config` とほぼ同じ引数を受け取ります。
これが意味するのは、 not found ビューには適用を制限するための述語を持た
せることができるということです。例えば:


.. code-block:: python
   :linenos:

   from pyramid.view import notfound_view_config

   @notfound_view_config(request_method='GET')
   def notfound_get(request):
       return Response('Not Found during GET, dude', status='404 Not Found')

   @notfound_view_config(request_method='POST')
   def notfound_post(request):
       return Response('Not Found during POST, dude', status='404 Not Found')

   def main(globals, **settings):
      config = Configurator()
      config.scan()


.. The ``notfound_get`` view will be called when a view could not be found and
.. the request method was ``GET``.  The ``notfound_post`` view will be called
.. when a view could not be found and the request method was ``POST``.

ビューが見つからず、リクエストメソッドが ``GET`` だった場合、
``notfound_get`` ビューが呼ばれるでしょう。ビューが見つからず、リクエスト
メソッドが ``POST`` だった場合、 ``notfound_post`` ビューが呼ばれるでしょう。


.. Like any other view, the notfound view must accept at least a ``request``
.. parameter, or both ``context`` and ``request``.  The ``request`` is the
.. current :term:`request` representing the denied action.  The ``context`` (if
.. used in the call signature) will be the instance of the
.. :exc:`~pyramid.httpexceptions.HTTPNotFound` exception that caused the view to
.. be called.

他のビューのように、 notfound ビューは少なくとも ``request`` パラメータ、
あるいは ``context`` および ``request`` の両方を受け取らなければなりません。
``request`` は拒否されたアクションを表わす現在の :term:`request` です。
(呼び出し署名の中で使用されていれば) ``context`` はビューを呼び出した
:exc:`~pyramid.httpexceptions.HTTPNotFound` 例外のインスタンスになるでしょう。


.. Both :meth:`pyramid.config.Configurator.add_notfound_view` and
.. :class:`pyramid.view.notfound_view_config` can be used to automatically
.. redirect requests to slash-appended routes. See
.. :ref:`redirecting_to_slash_appended_routes` for examples.

:meth:`pyramid.config.Configurator.add_notfound_view` と
:class:`pyramid.view.notfound_view_config` の両方は、リクエストを
スラッシュが追加された route に自動的にリダイレクトするために使用する
ことができます。
例については :ref:`redirecting_to_slash_appended_routes` を参照してください。


.. Here's some sample code that implements a minimal NotFound view callable:

これは、最小の NotFound ビュー callable を実装するサンプルコードです:


.. code-block:: python
   :linenos:

   from pyramid.httpexceptions import HTTPNotFound

   def notfound(request):
       return HTTPNotFound()


.. note::

   .. When a NotFound view callable is invoked, it is passed a
   .. :term:`request`.  The ``exception`` attribute of the request will be an
   .. instance of the :exc:`~pyramid.httpexceptions.HTTPNotFound` exception that
   .. caused the not found view to be called.  The value of
   .. ``request.exception.message`` will be a value explaining why the not found
   .. error was raised.  This message will be different when the
   .. ``pyramid.debug_notfound`` environment setting is true than it is when it
   .. is false.

   NotFound ビュー callable が起動される場合、 :term:`request` が渡されます。
   リクエストの ``exception`` 属性は、 not found ビューが呼び出される原因と
   なった :exc:`~pyramid.httpexceptions.HTTPNotFound` 例外のインスタンスに
   なります。 ``request.exception.message`` の値は、なぜ not found エラーが
   生じたのかを説明する値になります。 ``pyramid.debug_notfound`` 環境設定が
   true の場合と false の場合で、このメッセージは異なるでしょう。


.. note::

   .. Both :meth:`pyramid.config.Configurator.add_notfound_view` and
   .. :class:`pyramid.view.notfound_view_config` are new as of Pyramid 1.3.
   .. Older Pyramid documentation instructed users to use ``add_view`` instead,
   .. with a ``context`` of ``HTTPNotFound``.  This still works; the convenience
   .. method and decorator are just wrappers around this functionality.

   :meth:`pyramid.config.Configurator.add_notfound_view` と
   :class:`pyramid.view.notfound_view_config` の両方は Pyramid 1.3 から
   新しく追加されました。古い Pyramid ドキュメンテーションでは、代わりに
   ``context`` を ``HTTPNotFound`` として ``add_view`` を使うような指示が
   ありました。これは今でも動作します; 簡便なメソッドとデコレータは、
   この機能に対する単なるラッパーです。


.. warning::

   .. When a NotFound view callable accepts an argument list as
   .. described in :ref:`request_and_context_view_definitions`, the ``context``
   .. passed as the first argument to the view callable will be the
   .. :exc:`~pyramid.httpexceptions.HTTPNotFound` exception instance.  If
   .. available, the resource context will still be available as
   .. ``request.context``.

   NotFound ビュー callable が
   :ref:`request_and_context_view_definitions` に記述されているような
   引数リストを受け取った時、ビュー callable への最初の引数として渡される
   ``context`` は :exc:`~pyramid.httpexceptions.HTTPNotFound` 例外
   インスタンスになります。もしリソースコンテキストがあれば、それは
   依然として ``request.context`` として参照することができます。


.. index::
   single: forbidden view

.. Changing the Forbidden View

.. _changing_the_forbidden_view:

Forbidden View の変更
---------------------------

.. When :app:`Pyramid` can't authorize execution of a view based on the
.. :term:`authorization policy` in use, it invokes a :term:`forbidden view`.
.. The default forbidden response has a 403 status code and is very plain, but
.. the view which generates it can be overridden as necessary.

:app:`Pyramid` は、使用中の :term:`authorization policy` に基づいてビュー
の実行を認可することができない場合に :term:`forbidden view` を起動します。
デフォルトの fobidden レスポンスは 403 ステータスコードを持ち、非常に
簡素です。しかし、必要に応じて forbidden レスポンスを生成するビューを
オーバーライドすることができます。


.. The :term:`forbidden view` callable is a view callable like any other.  The
.. :term:`view configuration` which causes it to be a "forbidden" view consists
.. of using the :meth:`pyramid.config.Configurator.add_forbidden_view` API or the
.. :class:`pyramid.view.forbidden_view_config` decorator.

:term:`forbidden view` callable も他のものと同じようにビュー callable
の一つです。それを "forbidden" ビューにさせる :term:`view
configuration` は :meth:`pyramid.config.Configurator.add_forbidden_view`
API または :class:`pyramid.view.forbidden_view_config` デコレータの使用から
なります。


.. For example, you can add a forbidden view by using the
.. :meth:`pyramid.config.Configurator.add_forbidden_view` method to register a
.. forbidden view:

例えば、 forbidden ビューを登録する
:meth:`pyramid.config.Configurator.add_forbidden_view` メソッドを使用して
forbidden ビューを追加することができます:


.. code-block:: python
   :linenos:

   from helloworld.views import forbidden_view
   from pyramid.httpexceptions import HTTPForbidden
   config.add_forbidden_view(forbidden_view)


.. Replace ``helloworld.views.forbidden_view`` with a reference to the Python
.. :term:`view callable` you want to use to represent the Forbidden view.

``helloworld.views.forbidden_view`` を、 Forbidden ビューを表わすために
使用したい Python :term:`view callable` への参照に置き換えてください。


.. If instead you prefer to use decorators and a :term:`scan`, you can use the
.. :class:`pyramid.view.forbidden_view_config` decorator to mark a view callable
.. as a forbidden view:

あるいはデコレータと :term:`scan` を使用したければ、ビュー callable を
forbidden ビューとしてマークするために
:class:`pyramid.view.forbidden_view_config` デコレータを使用することが
できます:


.. code-block:: python
   :linenos:

   from pyramid.view import forbidden_view_config

   @forbidden_view_config()
   def forbidden(request):
       return Response('forbidden')

   def main(globals, **settings):
      config = Configurator()
      config.scan()


.. Like any other view, the forbidden view must accept at least a ``request``
.. parameter, or both ``context`` and ``request``.  If a forbidden view
.. callable accepts both ``context`` and ``request``, the HTTP Exception is passed
.. as context. The ``context`` as found by the router when view was
.. denied (that you normally would expect) is available as
.. ``request.context``.  The ``request`` is the  current :term:`request`
.. representing the denied action.

他のビューと同じように、 forbidden ビューは少なくとも ``request``
パラメータ、あるいは ``context`` および ``request`` の両方を受け取らなければ
なりません。 forbidden ビュー callable が ``context`` と ``request``
の両方を受け取る場合、 HTTP 例外がコンテキストとして渡されます。
(通常期待するような) ルーター (router) によって見つかった ``context`` は、
ビューが拒否された時には ``request.context`` として利用可能です。
``request`` は拒否されたアクションを表わす現在の :term:`request` です。


.. Here's some sample code that implements a minimal forbidden view:

これは、最小の forbidden ビューを実装するサンプルコードです:


.. code-block:: python
   :linenos:

   from pyramid.view import view_config
   from pyramid.response import Response

   def forbidden_view(request):
       return Response('forbidden')


.. note::

   .. When a forbidden view callable is invoked, it is passed a
   .. :term:`request`.  The ``exception`` attribute of the request will be an
   .. instance of the :exc:`~pyramid.httpexceptions.HTTPForbidden` exception
   .. that caused the forbidden view to be called.  The value of
   .. ``request.exception.message`` will be a value explaining why the forbidden
   .. was raised and ``request.exception.result`` will be extended information
   .. about the forbidden exception.  These messages will be different when the
   .. ``pyramid.debug_authorization`` environment setting is true than it is when
   .. it is false.

   forbidden ビュー callable が起動される場合、 :term:`request` が渡されます。
   request の ``exception`` 属性は forbidden ビューを呼び出した
   :exc:`~pyramid.httpexceptions.HTTPForbidden` 例外のインスタンスです。
   ``request.exception.message`` の値は、forbidden が発生した理由を
   説明する値になります。また ``request.exception.result`` は forbidden
   例外に関する拡張情報になります。
   ``pyramid.debug_authorization`` 環境設定が true の場合と false の場合で、
   これらのメッセージは異なるでしょう。


.. index::
   single: request factory

.. Changing the Request Factory

.. _changing_the_request_factory:

リクエストファクトリの変更
----------------------------

.. Whenever :app:`Pyramid` handles a request from a :term:`WSGI` server, it
.. creates a :term:`request` object based on the WSGI environment it has been
.. passed.  By default, an instance of the :class:`pyramid.request.Request`
.. class is created to represent the request object.

:app:`Pyramid` が :term:`WSGI` サーバからのリクエストを扱う場合は常に、
渡された WSGI 環境に基づいて :term:`request` オブジェクトが生成されます。
デフォルトでは、リクエストオブジェクトを表わすために
:class:`pyramid.request.Request` クラスのインスタンスが生成されます。


.. The class (aka "factory") that :app:`Pyramid` uses to create a request object
.. instance can be changed by passing a ``request_factory`` argument to the
.. constructor of the :term:`configurator`.  This argument can be either a
.. callable or a :term:`dotted Python name` representing a callable.

リクエストオブジェクトのインスタンスを生成するために :app:`Pyramid` が
使用するクラス (別名「ファクトリ」) は、 :term:`configurator` の
コンストラクタに ``request_factory`` 引数を渡すことで変更できます。
この引数は、 callable か、または callable を表す :term:`dotted Python name`
のいずれかです。


.. code-block:: python
   :linenos:

   from pyramid.request import Request

   class MyRequest(Request):
       pass

   config = Configurator(request_factory=MyRequest)


.. If you're doing imperative configuration, and you'd rather do it after you've
.. already constructed a :term:`configurator` it can also be registered via the
.. :meth:`pyramid.config.Configurator.set_request_factory` method:

命令型の設定を行っていて、 :term:`configurator` を構築した後に
行いたければ、 :meth:`pyramid.config.Configurator.set_request_factory`
メソッドによって登録することもできます:


.. code-block:: python
   :linenos:

   from pyramid.config import Configurator
   from pyramid.request import Request

   class MyRequest(Request):
       pass

   config = Configurator()
   config.set_request_factory(MyRequest)


.. index::
   single: before render event
   single: adding renderer globals

.. Using The Before Render Event

.. _beforerender_event:

Before Render イベントの使用
-----------------------------

.. Subscribers to the :class:`pyramid.events.BeforeRender` event may introspect
.. and modify the set of :term:`renderer globals` before they are passed to a
.. :term:`renderer`.  This event object iself has a dictionary-like interface
.. that can be used for this purpose.  For example:

:class:`pyramid.events.BeforeRender` イベントの subscriber は、
:term:`renderer globals` のセットが :term:`renderer` に渡される前に
検査して修正することができます。イベントオブジェクト自身には、この目的のために
使用することができる辞書風のインタフェースがあります。例えば:


.. code-block:: python
   :linenos:

    from pyramid.events import subscriber
    from pyramid.events import BeforeRender

    @subscriber(BeforeRender)
    def add_global(event):
        event['mykey'] = 'foo'


.. An object of this type is sent as an event just before a :term:`renderer` is
.. invoked (but *after* the application-level renderer globals factory added via
.. :class:`~pyramid.config.Configurator.set_renderer_globals_factory`, if any,
.. has injected its own keys into the renderer globals dictionary).

:term:`renderer` が起動される直前に (ただし
:class:`~pyramid.config.Configurator.set_renderer_globals_factory`
によって追加されたアプリケーションレベルのレンダラーグローバルファクトリが
もしあれば、それらがレンダラグローバル辞書にそれ自身のキーを注入した後で)
この型のオブジェクトがイベントとして送られます。


.. If a subscriber attempts to add a key that already exist in the renderer
.. globals dictionary, a :exc:`KeyError` is raised.  This limitation is enforced
.. because event subscribers do not possess any relative ordering.  The set of
.. keys added to the renderer globals dictionary by all
.. :class:`pyramid.events.BeforeRender` subscribers and renderer globals
.. factories must be unique.

subscriber がレンダラーグローバル辞書に既に存在するキーを追加しようとした
場合、 :exc:`KeyError` が発生します。イベント subscriber は相対順序を
保持しないので、この制限が強制されます。すべての
:class:`pyramid.events.BeforeRender` subscriber およびレンダラーグローバル
ファクトリによってレンダラーグローバル辞書に追加されるキーのセットは、
ユニークでなければなりません。


.. The dictionary returned from the view is accessible through the
.. :attr:`rendering_val` attribute of a :class:`~pyramid.events.BeforeRender`
.. event.

ビューから返された辞書は、 :class:`~pyramid.events.BeforeRender` イベントの
:attr:`rendering_val` 属性によってアクセスできます。


.. Suppose you return ``{'mykey': 'somevalue', 'mykey2': 'somevalue2'}`` from
.. your view callable, like so:

以下のようにビュー callable から
``{'mykey': 'somevalue', 'mykey2': 'somevalue2'}`` を返すことを考えてください:


.. code-block:: python
   :linenos:

   from pyramid.view import view_config

   @view_config(renderer='some_renderer')
   def myview(request):
       return {'mykey': 'somevalue', 'mykey2': 'somevalue2'}


.. :attr:`rendering_val` can be used to access these values from the
.. :class:`~pyramid.events.BeforeRender` object:

:class:`~pyramid.events.BeforeRender` オブジェクトからこれらの値に
アクセスするために :attr:`rendering_val` を使うことができます:


.. code-block:: python
   :linenos:

   from pyramid.events import subscriber
   from pyramid.events import BeforeRender

   @subscriber(BeforeRender)
   def read_return(event):
       # {'mykey': 'somevalue'} is returned from the view
       print(event.rendering_val['mykey'])


.. See the API documentation for the :class:`~pyramid.events.BeforeRender` event
.. interface at :class:`pyramid.interfaces.IBeforeRender`.

:class:`~pyramid.events.BeforeRender` イベントインタフェースについては
:class:`pyramid.interfaces.IBeforeRender` の API ドキュメンテーションを
参照してください。


.. Another (deprecated) mechanism which allows event subscribers more control
.. when adding renderer global values exists in :ref:`adding_renderer_globals`.

レンダラーグローバルに値を追加する際に、イベント subscriber により多くの
コントロールを与える別の (非推奨の) メカニズムが
:ref:`adding_renderer_globals` にあります。


.. index::
   single: adding renderer globals

.. Adding Renderer Globals (Deprecated)

.. _adding_renderer_globals:

レンダラーグローバルの追加 (非推奨)
------------------------------------

.. .. warning:: this feature is deprecated as of Pyramid 1.1.  A non-deprecated
..    mechanism which allows event subscribers to add renderer global values
..    is documented in :ref:`beforerender_event`.

.. warning:: 

   この機能は Pyramid 1.1 から非推奨になりました。イベント subscriber が
   レンダラーグローバルに値を追加できるようにするための推奨される
   メカニズムは :ref:`beforerender_event` の中で文書化されます。


.. Whenever :app:`Pyramid` handles a request to perform a rendering (after a
.. view with a ``renderer=`` configuration attribute is invoked, or when any of
.. the methods beginning with ``render`` within the :mod:`pyramid.renderers`
.. module are called), *renderer globals* can be injected into the *system*
.. values sent to the renderer.  By default, no renderer globals are injected,
.. and the "bare" system values (such as ``request``, ``context``, ``view``, and
.. ``renderer_name``) are the only values present in the system dictionary
.. passed to every renderer.

:app:`Pyramid` がレンダリングを実行するリクエストを扱う場合
(``renderer=`` 設定属性を持つビューが起動された後、あるいは
:mod:`pyramid.renderers` モジュール内の ``render`` で始まるメソッドの
うちのいずれかが呼ばれた場合) は常に、 *レンダラーグローバル* をレンダラー
に送られる *システム* 値に注入することができます。デフォルトではレンダラー
グローバルは注入されず、 (``request``, ``context``, ``view``,
``renderer_name`` のような) 「素の」システム値が、すべてのレンダラーに
渡されるシステム辞書の中にある唯一の値です。


.. A callback that :app:`Pyramid` will call every time a renderer is invoked can
.. be added by passing a ``renderer_globals_factory`` argument to the
.. constructor of the :term:`configurator`.  This callback can either be a
.. callable object or a :term:`dotted Python name` representing such a callable.

:term:`configurator` のコンストラクタに ``renderer_globals_factory`` 引数を
渡すことにより、レンダラーが起動される度に :app:`Pyramid` が呼び出す
コールバックを追加することができます。このコールバックは、 callable
オブジェクト、あるいはそのような callable を表す :term:`dotted Python name`
のいずれかです。


.. code-block:: python
   :linenos:

   def renderer_globals_factory(system):
       return {'a': 1}

   config = Configurator(
            renderer_globals_factory=renderer_globals_factory)


.. Such a callback must accept a single positional argument (notionally named
.. ``system``) which will contain the original system values.  It must return a
.. dictionary of values that will be merged into the system dictionary.  See
.. :ref:`renderer_system_values` for description of the values present in the
.. system dictionary.

このようなコールバックは、オリジナルのシステム値を含む単一の位置引数
(名目上 ``system`` という名前が付けられます) を受け取らなければなりません。
それは、システム辞書にマージされる値の辞書を返さなければなりません。
システム辞書の中にある値の説明については、
:ref:`renderer_system_values` を参照してください。


.. If you're doing imperative configuration, and you'd rather do it after you've
.. already constructed a :term:`configurator` it can also be registered via the
.. :meth:`pyramid.config.Configurator.set_renderer_globals_factory` method:

命令型の設定を行っていて、 :term:`configurator` を構築した後に
行いたければ、 :meth:`pyramid.config.Configurator.set_renderer_globals_factory`
メソッドによって登録することもできます:


.. code-block:: python
   :linenos:

   from pyramid.config import Configurator

   def renderer_globals_factory(system):
       return {'a': 1}

   config = Configurator()
   config.set_renderer_globals_factory(renderer_globals_factory)


.. index::
   single: response callback

.. Using Response Callbacks

.. _using_response_callbacks:

レスポンスコールバックの使用
----------------------------

.. Unlike many other web frameworks, :app:`Pyramid` does not eagerly create a
.. global response object.  Adding a :term:`response callback` allows an
.. application to register an action to be performed against whatever response
.. object is returned by a view, usually in order to mutate the response.

他の多くのウェブフレームワークと異なり、 :app:`Pyramid` は積極的に
グローバルなレスポンスオブジェクトを生成しません。
アプリケーションは :term:`response callback` を追加することによって、
ビューから返されるすべてのレスポンスオブジェクトに対して実行されるアクションを
(通常レスポンスを変更するために) 登録できます。


.. The :meth:`pyramid.request.Request.add_response_callback` method is used to
.. register a response callback.

:meth:`pyramid.request.Request.add_response_callback` メソッドが
レスポンスコールバックを登録するために使用されます。


.. A response callback is a callable which accepts two positional parameters:
.. ``request`` and ``response``.  For example:

レスポンスコールバックは、 2 つの位置パラメータ ``request`` と
``response`` を受け取る callable です。例えば:


.. code-block:: python
   :linenos:

   def cache_callback(request, response):
       """Set the cache_control max_age for the response"""
       if request.exception is not None:
           response.cache_control.max_age = 360
   request.add_response_callback(cache_callback)


.. No response callback is called if an unhandled exception happens in
.. application code, or if the response object returned by a :term:`view
.. callable` is invalid.  Response callbacks *are*, however, invoked when a
.. :term:`exception view` is rendered successfully: in such a case, the
.. :attr:`request.exception` attribute of the request when it enters a response
.. callback will be an exception object instead of its default value of
.. ``None``.

未処理の例外がアプリケーションコードの中で起こった場合、あるいは
:term:`view callable` によって返されたレスポンスオブジェクトが無効の
場合、レスポンスコールバックは呼ばれません。しかし、 :term:`exception
view` が正常にレンダリングされる場合、レスポンスコールバックは起動
*されます*: そのような場合、レスポンスコールバックが呼ばれた時の
リクエストの :attr:`request.exception` 属性は、デフォルト値 ``None`` の
代わりに例外オブジェクトになります。


.. Response callbacks are called in the order they're added
.. (first-to-most-recently-added).  All response callbacks are called *after*
.. the :class:`~pyramid.events.NewResponse` event is sent.  Errors raised by
.. response callbacks are not handled specially.  They will be propagated to the
.. caller of the :app:`Pyramid` router application.

レスポンスコールバックは、追加された順番で
(first-to-most-recently-added; 最後に追加されたものが最初に) 呼ばれます。
:class:`~pyramid.events.NewResponse` イベントが送られた *後で* 、すべて
のレスポンスコールバックが呼ばれます。レスポンスコールバックで発生した
エラーは特別な扱いは受けません。それは呼び出し元の :app:`Pyramid` ルーター
アプリケーションに伝搬するでしょう。


.. A response callback has a lifetime of a *single* request.  If you want a
.. response callback to happen as the result of *every* request, you must
.. re-register the callback into every new request (perhaps within a subscriber
.. of a :class:`~pyramid.events.NewRequest` event).

レスポンスコールバックは、単一リクエストのライフタイムを持ちます。
*すべての* リクエストの結果としてレスポンスコールバックが起こることを
期待する場合、新しく作られるすべてのリクエストにコールバックを再登録
しなければなりません (恐らく :class:`~pyramid.events.NewRequest`
イベントの subscriber 内で)。


.. index::
   single: finished callback

.. Using Finished Callbacks

.. _using_finished_callbacks:

finished コールバックの使用
---------------------------

.. A :term:`finished callback` is a function that will be called unconditionally
.. by the :app:`Pyramid` :term:`router` at the very end of request processing.
.. A finished callback can be used to perform an action at the end of a request
.. unconditionally.

:term:`finished callback` は、 :app:`Pyramid` :term:`router` によって
リクエスト処理の最後に無条件で呼ばれる関数です。 finished コールバックは、
リクエストの終わりに無条件でアクションを行なうために使用することができます。


.. The :meth:`pyramid.request.Request.add_finished_callback` method is used to
.. register a finished callback.

:meth:`pyramid.request.Request.add_finished_callback` メソッドは、
finished コールバックを登録するために使用されます。


.. A finished callback is a callable which accepts a single positional
.. parameter: ``request``.  For example:

finished コールバックは、単一の位置パラメータ ``request`` を受け取る
callable です。例えば:


.. code-block:: python
   :linenos:

   import logging

   log = logging.getLogger(__name__)

   def log_callback(request):
       """Log information at the end of request"""
       log.debug('Request is finished.')
   request.add_finished_callback(log_callback)


.. Finished callbacks are called in the order they're added
.. (first-to-most-recently-added).  Finished callbacks (unlike a
.. :term:`response callback`) are *always* called, even if an exception
.. happens in application code that prevents a response from being
.. generated.

finished コールバックは、追加された順番で
(first-to-most-recently-added; 最後に追加されたものが最初に) 呼ばれます。
(:term:`response callback` と異なり) finished コールバックは、
レスポンスが生成されるのを妨げる例外がアプリケーションコード内で
起こっても常に呼ばれます。


.. The set of finished callbacks associated with a request are called *very
.. late* in the processing of that request; they are essentially the very last
.. thing called by the :term:`router` before a request "ends". They are called
.. after response processing has already occurred in a top-level ``finally:``
.. block within the router request processing code.  As a result, mutations
.. performed to the ``request`` provided to a finished callback will have no
.. meaningful effect, because response processing will have already occurred,
.. and the request's scope will expire almost immediately after all finished
.. callbacks have been processed.

リクエストに関連付けられた finished コールバックのセットは、そのリクエスト
を処理する *最終段階で* 呼ばれます; それらは、本質的にリクエストが「終わる」
前に :term:`router` によって呼ばれるまさに最後のものです。レスポンス処理が
既に生じた後、 ルーターのリクエスト処理コード内のトップレベルの ``finally:``
ブロックでそれらが呼ばれます。その結果、レスポンス処理は既に終わって
いるため、 finished コールバックに渡された ``request`` に対して行なった
変更には意味のある効果はありません。また、リクエストのスコープはすべての
finished コールバックが処理された後でほとんど直ちに終了します。


.. Errors raised by finished callbacks are not handled specially.  They
.. will be propagated to the caller of the :app:`Pyramid` router
.. application.

finished コールバックで発生したエラーは特別な扱いは受けません。それは
呼び出し元の :app:`Pyramid` ルーターアプリケーションに伝搬するでしょう。


.. A finished callback has a lifetime of a *single* request.  If you want a
.. finished callback to happen as the result of *every* request, you must
.. re-register the callback into every new request (perhaps within a subscriber
.. of a :class:`~pyramid.events.NewRequest` event).

finished コールバックは、単一リクエストのライフタイムを持ちます。
*すべての* リクエストの結果として finished コールバックが起こることを
期待する場合、すべての新しく作られるリクエストにコールバックを再登録
しなければなりません (恐らく :class:`~pyramid.events.NewRequest`
イベントの subscriber 内で)。


.. index::
   single: traverser

.. Changing the Traverser

.. _changing_the_traverser:

トラバーサーの変更
----------------------

.. The default :term:`traversal` algorithm that :app:`Pyramid` uses is explained
.. in :ref:`traversal_algorithm`.  Though it is rarely necessary, this default
.. algorithm can be swapped out selectively for a different traversal pattern
.. via configuration.

:app:`Pyramid` が使用するデフォルトの :term:`traversal` アルゴリズムは
:ref:`traversal_algorithm` の中で説明されています。必要になることは
めったにありませんが、設定によってこのデフォルトのアルゴリズムを異なる
トラバーサルパターンと選択的に交換することができます。


.. code-block:: python
   :linenos:

   from pyramid.config import Configurator
   from myapp.traversal import Traverser
   config = Configurator()
   config.add_traverser(Traverser)


.. In the example above, ``myapp.traversal.Traverser`` is assumed to be a class
.. that implements the following interface:

上記の例において、 ``myapp.traversal.Traverser`` は次のインタフェースを
実装するクラスであると仮定されます:


.. code-block:: python
   :linenos:

   class Traverser(object):
       def __init__(self, root):
           """ Accept the root object returned from the root factory """

       def __call__(self, request):
           """ Return a dictionary with (at least) the keys ``root``,
           ``context``, ``view_name``, ``subpath``, ``traversed``,
           ``virtual_root``, and ``virtual_root_path``.  These values are
           typically the result of a resource tree traversal.  ``root``
           is the physical root object, ``context`` will be a resource
           object, ``view_name`` will be the view name used (a Unicode
           name), ``subpath`` will be a sequence of Unicode names that
           followed the view name but were not traversed, ``traversed``
           will be a sequence of Unicode names that were traversed
           (including the virtual root path, if any) ``virtual_root``
           will be a resource object representing the virtual root (or the
           physical root if traversal was not performed), and
           ``virtual_root_path`` will be a sequence representing the
           virtual root path (a sequence of Unicode names) or None if
           traversal was not performed.

           Extra keys for special purpose functionality can be added as
           necessary.

           All values returned in the dictionary will be made available
           as attributes of the ``request`` object.
           """


.. More than one traversal algorithm can be active at the same time.  For
.. instance, if your :term:`root factory` returns more than one type of object
.. conditionally, you could claim that an alternate traverser adapter is "for"
.. only one particular class or interface.  When the root factory returned an
.. object that implemented that class or interface, a custom traverser would be
.. used.  Otherwise, the default traverser would be used.  For example:

複数のトラバーサルアルゴリズムを同時に有効にすることもできます。例えば、
:term:`root factory` が条件付きで複数の型のオブジェクトを返す場合、
代替トラバーサーアダプタをある特定のクラスあるいはインタフェース
「専用」と宣言することができます。root ファクトリがそのクラスまたは
インタフェースを実装したオブジェクトを返した場合、カスタムトラバーサーが
使用されます。そうでなければ、デフォルトトラバーサーが使用されます。例えば:


.. code-block:: python
   :linenos:

   from myapp.traversal import Traverser
   from myapp.resources import MyRoot
   from pyramid.config import Configurator
   config = Configurator()
   config.add_traverser(Traverser, MyRoot)


.. If the above stanza was added to a Pyramid ``__init__.py`` file's ``main``
.. function, :app:`Pyramid` would use the ``myapp.traversal.Traverser`` only
.. when the application :term:`root factory` returned an instance of the
.. ``myapp.resources.MyRoot`` object.  Otherwise it would use the default
.. :app:`Pyramid` traverser to do traversal.

もし上記のコード断片が Pyramid ``__init__.py`` ファイルの ``main`` 関数に
追加されれば、 :app:`Pyramid` は アプリケーション :term:`root factory` が
``myapp.resources.MyRoot`` オブジェクトのインスタンスを返した時にだけ
``myapp.traversal.Traverser`` を使用します。そうでなければ、トラバーサルを
行うためにデフォルトの :app:`Pyramid` トラバーサーが使用されます。


.. index::
   single: url generator

.. Changing How :meth:`pyramid.request.Request.resource_url` Generates a URL

.. _changing_resource_url:

:meth:`pyramid.request.Request.resource_url` の URL 生成方法の変更
-------------------------------------------------------------------------

.. When you add a traverser as described in :ref:`changing_the_traverser`, it's
.. often convenient to continue to use the
.. :meth:`pyramid.request.Request.resource_url` API.  However, since the way
.. traversal is done will have been modified, the URLs it generates by default
.. may be incorrect when used against resources derived from your custom
.. traverser.

:ref:`changing_the_traverser` で説明されるようなトラバーサーを追加する場合、
:meth:`pyramid.request.Request.resource_url` API を使用し続けることは
多くの場合有用です。しかし、この API がカスタムトラバーサーに由来する
リソースに対して使用された場合、トラバーサルが終了する方法が修正されて
いるためデフォルトで生成される URL は正しくないかもしれません。


.. If you've added a traverser, you can change how
.. :meth:`~pyramid.request.Request.resource_url` generates a URL for a specific
.. type of resource by adding a call to
.. :meth:`pyramid.config.add_resource_url_adapter`.

トラバーサーを追加している場合、
:meth:`pyramid.config.add_resource_url_adapter` 呼び出しを追加することに
よって :meth:`~pyramid.request.Request.resource_url` が特定の型の
リソースに対する URL を生成する方法を変更できます。


.. For example:

例えば:


.. code-block:: python
   :linenos:

   from myapp.traversal import ResourceURLAdapter
   from myapp.resources import MyRoot

   config.add_resource_url_adapter(ResourceURLAdapter, MyRoot)


.. In the above example, the ``myapp.traversal.ResourceURLAdapter`` class will
.. be used to provide services to :meth:`~pyramid.request.Request.resource_url`
.. any time the :term:`resource` passed to ``resource_url`` is of the class
.. ``myapp.resources.MyRoot``.  The ``resource_iface`` argument ``MyRoot``
.. represents the type of interface that must be possessed by the resource for
.. this resource url factory to be found.  If the ``resource_iface`` argument is
.. omitted, this resource url adapter will be used for *all* resources.

上記の例において、 ``resource_url`` に渡された :term:`resource` がクラス
``myapp.resources.MyRoot`` である場合は常に、
:meth:`~pyramid.request.Request.resource_url` にサービスを提供するために
``myapp.traversal.ResourceURLAdapter`` クラスが使用されます。
``resource_iface`` の引数 ``MyRoot`` は、見つかったリソース url ファクトリ
に対してリソースによって所有されなければならないインタフェースの型を
表わしています。 ``resource_iface`` 引数が省略されれば、このリソース
url アダプターはすべてのリソースに使用されます。


.. The API that must be implemented by your a class that provides
.. :class:`~pyramid.interfaces.IResourceURL` is as follows:

:class:`~pyramid.interfaces.IResourceURL` を提供するクラスで実装
しなければならない API は以下の通りです:


.. code-block:: python
  :linenos:

  class MyResourceURL(object):
      """ An adapter which provides the virtual and physical paths of a
          resource
      """
      def __init__(self, resource, request):
          """ Accept the resource and request and set self.physical_path and 
          self.virtual_path"""
          self.virtual_path =  some_function_of(resource, request)
          self.physical_path =  some_other_function_of(resource, request)


.. The default context URL generator is available for perusal as the class
.. :class:`pyramid.traversal.ResourceURL` in the `traversal module
.. <http://github.com/Pylons/pyramid/blob/master/pyramid/traversal.py>`_ of the
.. :term:`Pylons` GitHub Pyramid repository.

デフォルトのコンテキスト URL ジェネレータを熟読したければ、
:term:`Pylons` GitHub Pyramid リポジトリの `traversal module
<http://github.com/Pylons/pyramid/blob/master/pyramid/traversal.py>`_
に含まれるクラス :class:`pyramid.traversal.ResourceURL` が利用可能です。


.. See :meth:`pyramid.config.add_resource_url_adapter` for more information.

詳細は :meth:`pyramid.config.add_resource_url_adapter` を参照してください。


.. index::
   single: IResponse
   single: special view responses

.. Changing How Pyramid Treats View Responses

.. _using_iresponse:

Pyramid がビューレスポンスを扱う方法の変更
------------------------------------------

.. It is possible to control how Pyramid treats the result of calling a view
.. callable on a per-type basis by using a hook involving
.. :meth:`pyramid.config.Configurator.add_response_adapter` or the
.. :class:`~pyramid.response.response_adapter` decorator.

:meth:`pyramid.config.Configurator.add_response_adapter` あるいは
:class:`~pyramid.response.response_adapter` デコレータに関係するフック
を使うことにより、 Pyramid がビュー callable を呼び出した結果をどのように
扱うかを型に応じてコントロールすることが可能です。


.. .. note:: This feature is new as of Pyramid 1.1.

.. note:: これは Pyramid 1.1 からの新機能です。


.. Pyramid, in various places, adapts the result of calling a view callable to
.. the :class:`~pyramid.interfaces.IResponse` interface to ensure that the
.. object returned by the view callable is a "true" response object.  The vast
.. majority of time, the result of this adaptation is the result object itself,
.. as view callables written by "civilians" who read the narrative documentation
.. contained in this manual will always return something that implements the
.. :class:`~pyramid.interfaces.IResponse` interface.  Most typically, this will
.. be an instance of the :class:`pyramid.response.Response` class or a subclass.
.. If a civilian returns a non-Response object from a view callable that isn't
.. configured to use a :term:`renderer`, he will typically expect the router to
.. raise an error.  However, you can hook Pyramid in such a way that users can
.. return arbitrary values from a view callable by providing an adapter which
.. converts the arbitrary return value into something that implements
.. :class:`~pyramid.interfaces.IResponse`.

Pyramid は、ビュー callable が返したオブジェクトが「本物の」レスポンス
オブジェクトであることを保証するために、様々な場所でビュー callable を
呼び出した結果を :class:`~pyramid.interfaces.IResponse` インタフェースに
適合させます。ほとんどの場合、この適合の結果は結果のオブジェクト自体です。
なぜなら、このマニュアルに含まれるドキュメンテーションを読んだ
「一般ユーザ」によって書かれたビュー callable は、常に
:class:`~pyramid.interfaces.IResponse` インタフェースを実装する
オブジェクトを返すからです。最も典型的には、これは
:class:`pyramid.response.Response` クラスあるいはそのサブクラスの
インスタンスになるでしょう。一般のユーザが :term:`renderer` を使用する
ように設定されていないビュー callable から非レスポンスオブジェクトを
返す場合、そのユーザは、典型的にはルーターがエラーを発生させることを
期待するでしょう。しかしながら、任意の戻り値から
:class:`~pyramid.interfaces.IResponse` を実装するオブジェクトに変換する
アダプターを提供することにより、ユーザがビュー callable から任意の値を
返せるように Pyramid をフックすることができます。


.. For example, if you'd like to allow view callables to return bare string
.. objects (without requiring a :term:`renderer` to convert a string to a
.. response object), you can register an adapter which converts the string to a
.. Response:

例えば、ビュー callable が (文字列をレスポンスオブジェクトに変換する
:term:`renderer` を必要とせずに) 裸の文字列オブジェクトを返せるように
したければ、文字列をレスポンスに変換するアダプターを登録することができます:


.. code-block:: python
   :linenos:

   from pyramid.response import Response

   def string_response_adapter(s):
       response = Response(s)
       return response

   # config is an instance of pyramid.config.Configurator

   config.add_response_adapter(string_response_adapter, str)


.. Likewise, if you want to be able to return a simplified kind of response
.. object from view callables, you can use the IResponse hook to register an
.. adapter to the more complex IResponse interface:

同様に、ビュー callable から単純化されたレスポンスオブジェクトの一種を
返せるようにしたければ、より複雑な IResponse インタフェースへの
アダプターを登録するために IResponse フックを使用することができます:


.. code-block:: python
   :linenos:

   from pyramid.response import Response

   class SimpleResponse(object):
       def __init__(self, body):
           self.body = body

   def simple_response_adapter(simple_response):
       response = Response(simple_response.body)
       return response

   # config is an instance of pyramid.config.Configurator

   config.add_response_adapter(simple_response_adapter, SimpleResponse)


.. If you want to implement your own Response object instead of using the
.. :class:`pyramid.response.Response` object in any capacity at all, you'll have
.. to make sure the object implements every attribute and method outlined in
.. :class:`pyramid.interfaces.IResponse` and you'll have to ensure that it uses
.. ``zope.interface.implementer(IResponse)`` as a class decoratoror.

いずれの場合であっても、 :class:`pyramid.response.Response` オブジェクト
を使用する代わりに自分のレスポンスオブジェクトを実装したければ、
:class:`pyramid.interfaces.IResponse` の中で概説されているすべての
属性とメソッドをそのオブジェクトが実装していることを保証しなければなりません。
また、クラスデコレータとして ``zope.interface.implementer(IResponse)``
を使用する必要があります。


.. code-block:: python
   :linenos:

   from pyramid.interfaces import IResponse
   from zope.interface import implementer

   @implementer(IResponse)
   class MyResponse(object):
       # ... an implementation of every method and attribute 
       # documented in IResponse should follow ...


.. When an alternate response object implementation is returned by a view
.. callable, if that object asserts that it implements
.. :class:`~pyramid.interfaces.IResponse` (via
.. ``zope.interface.implementer(IResponse)``) , an adapter needn't be registered
.. for the object; Pyramid will use it directly.

代替レスポンスオブジェクトの実装がビュー callable によって返された場合、
そのオブジェクトが (``zope.interface.implementer(IResponse)`` によって)
:class:`~pyramid.interfaces.IResponse` を実装すると主張するなら、その
オブジェクトのためにアダプターを登録する必要はありません; Pyramid は
それを直接使用するでしょう。


.. An IResponse adapter for ``webob.Response`` (as opposed to
.. :class:`pyramid.response.Response`) is registered by Pyramid by default at
.. startup time, as by their nature, instances of this class (and instances of
.. subclasses of the class) will natively provide IResponse.  The adapter
.. registered for ``webob.Response`` simply returns the response object.

``webob.Response`` のための IResponse アダプターは、
(:class:`pyramid.response.Response` とは対照的に) Pyramid によって
スタートアップ時にデフォルトで登録されます。なぜなら、このクラスの
インスタンス (またこのクラスのサブクラスのインスタンス) はその性質上
IResponse をネイティブに提供するからです。 ``webob.Response`` に対して
登録されたアダプターは、単にレスポンスオブジェクトを返します。


.. Instead of using :meth:`pyramid.config.Configurator.add_response_adapter`,
.. you can use the :class:`pyramid.response.response_adapter` decorator:

:meth:`pyramid.config.Configurator.add_response_adapter` を使用する代わりに、
:class:`pyramid.response.response_adapter` デコレータを使用しても構いません:


.. code-block:: python
   :linenos:

   from pyramid.response import Response
   from pyramid.response import response_adapter

   @response_adapter(str)
   def string_response_adapter(s):
       response = Response(s)
       return response


.. The above example, when scanned, has the same effect as:

上記の例は、 scan された時に以下と同じ効果があります:


.. code-block:: python

   config.add_response_adapter(string_response_adapter, str)


.. The :class:`~pyramid.response.response_adapter` decorator will have no effect
.. until activated by a :term:`scan`.

:term:`scan` によって活性化されるまで、
:class:`~pyramid.response.response_adapter` デコレータは効果がないでしょう。


.. index::
   single: view mapper

.. Using a View Mapper

.. _using_a_view_mapper:

ビューマッパーの変更
--------------------

.. The default calling conventions for view callables are documented in the
.. :ref:`views_chapter` chapter.  You can change the way users define view
.. callables by employing a :term:`view mapper`.

ビュー callable のためのデフォルトの呼び出し規則は
:ref:`views_chapter` 章の中で文書化されます。 :term:`view mapper` を
使用することで、ユーザがビュー callable を定義する方法を変更できます。


.. A view mapper is an object that accepts a set of keyword arguments and which
.. returns a callable.  The returned callable is called with the :term:`view
.. callable` object.  The returned callable should itself return another
.. callable which can be called with the "internal calling protocol" ``(context,
.. request)``.

ビューマッパーは、いくつかのキーワード引数を受け取って 1 つの callable
を返すオブジェクトです。返された callable は :term:`view callable`
オブジェクトと共に呼び出されます。返された callable は、それ自体
「内部呼び出しプロトコル」 ``(context, request)`` で呼び出すことのできる
別の callable を返す必要があります。


.. You can use a view mapper in a number of ways:

複数の方法でビューマッパーを使用することができます:


.. - by setting a ``__view_mapper__`` attribute (which is the view mapper
..   object) on the view callable itself

.. - by passing the mapper object to
..   :meth:`pyramid.config.Configurator.add_view` (or its declarative/decorator
..   equivalents) as the ``mapper`` argument.

.. - by registering a *default* view mapper.


- ビュー callable 自体に ``__view_mapper__`` 属性
  (それはビューマッパーオブジェクトです) をセットすることによって。

- マッパーオブジェクトを :meth:`pyramid.config.Configurator.add_view`
  (あるいはその宣言的/デコレータ等価物) に ``mapper`` 引数として渡す
  ことによって。

- *デフォルト* ビューマッパーを登録することによって。


.. Here's an example of a view mapper that emulates (somewhat) a Pylons
.. "controller".  The mapper is initialized with some keyword arguments.  Its
.. ``__call__`` method accepts the view object (which will be a class).  It uses
.. the ``attr`` keyword argument it is passed to determine which attribute
.. should be used as an action method.  The wrapper method it returns accepts
.. ``(context, request)`` and returns the result of calling the action method
.. with keyword arguments implied by the :term:`matchdict` after popping the
.. ``action`` out of it.  This somewhat emulates the Pylons style of calling
.. action methods with routing parameters pulled out of the route matching dict
.. as keyword arguments.

これは Pylons 「コントローラー」を (ある程度) 模倣するビューマッパーの
一例です。マッパーはいくつかのキーワード引数で初期化されます。その
``__call__`` メソッドはビューオブジェクト (それはクラスになるでしょう)
を受け取ります。それは、アクションメソッドとして使用すべき属性を決定す
るために、渡される ``attr`` キーワード引数を使用します。それが返す
ラッパーメソッドは、 ``(context, request)`` を受け取り、 :term:`matchdict`
から ``action`` をポップした後で :term:`matchdict` によって示唆された
キーワード引数を伴ってアクションメソッドを呼び出した結果を返します。これは、
route マッチ辞書から取り出されたルーティングパラメータをキーワード引数
としてアクションメソッドを呼び出す Pylons スタイルをある程度エミュレート
します。


.. code-block:: python
   :linenos:

   # framework

   class PylonsControllerViewMapper(object):
       def __init__(self, **kw):
           self.kw = kw

       def __call__(self, view):
           attr = self.kw['attr']
           def wrapper(context, request):
               matchdict = request.matchdict.copy()
               matchdict.pop('action', None)
               inst = view(request)
               meth = getattr(inst, attr)
               return meth(**matchdict)
           return wrapper

   class BaseController(object):
       __view_mapper__ = PylonsControllerViewMapper


.. A user might make use of these framework components like so:

ユーザは、これらのフレームワークコンポーネントを以下のように使用
するでしょう:


.. code-block:: python
   :linenos:

   # user application

   from pyramid.response import Response
   from pyramid.config import Configurator
   import pyramid_handlers
   from wsgiref.simple_server import make_server

   class MyController(BaseController):
       def index(self, id):
           return Response(id)

   if __name__ == '__main__':
       config = Configurator()
       config.include(pyramid_handlers)
       config.add_handler('one', '/{id}', MyController, action='index')
       config.add_handler('two', '/{action}/{id}', MyController)
       server.make_server('0.0.0.0', 8080, config.make_wsgi_app())
       server.serve_forever()


.. The :meth:`pyramid.config.Configurator.set_view_mapper` method can be used to
.. set a *default* view mapper (overriding the superdefault view mapper used by
.. Pyramid itself).

:meth:`pyramid.config.Configurator.set_view_mapper` メソッドは、
(Pyramid 自体によって使用されるスーパーデフォルトビューマッパーをオーバーライド
して) *デフォルト* ビューマッパーをセットするために使用することができます。


.. A *single* view registration can use a view mapper by passing the mapper as
.. the ``mapper`` argument to :meth:`~pyramid.config.Configuration.add_view`.

*単一の* ビュー登録は、マッパーを ``mapper`` 引数として
:meth:`~pyramid.config.Configuration.add_view` へ渡すことによって、
ビューマッパーを使用することができます。


.. index::
   single: configuration decorator

.. Registering Configuration Decorators

.. _registering_configuration_decorators:

設定デコレータの登録
------------------------------------

.. Decorators such as :class:`~pyramid.view.view_config` don't change the
.. behavior of the functions or classes they're decorating.  Instead, when a
.. :term:`scan` is performed, a modified version of the function or class is
.. registered with :app:`Pyramid`.

:class:`~pyramid.view.view_config` のようなデコレータは、それがデコレート
する関数またはクラスの振る舞いを変更しません。代わりに、 :term:`scan` が
実行された時に、関数またはクラスの修正版が :app:`Pyramid` に登録されます。


.. You may wish to have your own decorators that offer such behaviour. This is
.. possible by using the :term:`Venusian` package in the same way that it is
.. used by :app:`Pyramid`.

そのような振る舞いを提供する自分のデコレータが欲しいと思うかもしれません。
これは、 :app:`Pyramid` が行っているのと同じ方法で
:term:`Venusian` パッケージを使用することにより実現可能です。


.. By way of example, let's suppose you want to write a decorator that registers
.. the function it wraps with a :term:`Zope Component Architecture` "utility"
.. within the :term:`application registry` provided by :app:`Pyramid`. The
.. application registry and the utility inside the registry is likely only to be
.. available once your application's configuration is at least partially
.. completed. A normal decorator would fail as it would be executed before the
.. configuration had even begun.

例として、関数を登録するデコレータを書きたいと仮定しましょう。その
デコレータは、 :app:`Pyramid` によって提供される :term:`application
registry` の中に :term:`Zope Component Architecture` 「ユーティリティ」
としてそれがラップする関数を登録します。アプリケーションレジストリおよび
レジストリ内部のユーティリティは、アプリケーション設定が少なくとも
部分的に完了してから初めて利用可能になります。設定が開始される前にそれが
実行された場合、通常のデコレータは失敗します。


.. However, using :term:`Venusian`, the decorator could be written as
.. follows:

しかし、 :term:`Venusian` を使用すれば以下のようにデコレータを書く
ことができます:


.. code-block:: python
   :linenos:

   import venusian
   from mypackage.interfaces import IMyUtility

   class registerFunction(object):

       def __init__(self, path):
           self.path = path

       def register(self, scanner, name, wrapped):
           registry = scanner.config.registry
           registry.getUtility(IMyUtility).register(
               self.path, wrapped)

       def __call__(self, wrapped):
           venusian.attach(wrapped, self.register)
           return wrapped


.. This decorator could then be used to register functions throughout
.. your code:

その後、このデコレータを使用してコードのどこでも関数を登録することが
できます:


.. code-block:: python
   :linenos:

   @registerFunction('/some/path')
   def my_function():
      do_stuff()


.. However, the utility would only be looked up when a :term:`scan` was
.. performed, enabling you to set up the utility in advance:

しかし、事前にユーティリティをセットアップできるにもかかわらず、
:term:`scan` が行なわれた時にだけユーティリティが検索されます:


.. code-block:: python
   :linenos:

   from zope.interface import implementer

   from wsgiref.simple_server import make_server
   from pyramid.config import Configurator
   from mypackage.interfaces import IMyUtility

   @implementer(IMyUtility)
   class UtilityImplementation:

       def __init__(self):
          self.registrations = {}

       def register(self, path, callable_):
          self.registrations[path] = callable_

   if __name__ == '__main__':
       config = Configurator()
       config.registry.registerUtility(UtilityImplementation())
       config.scan()
       app = config.make_wsgi_app()
       server = make_server('0.0.0.0', 8080, app)
       server.serve_forever()


.. For full details, please read the `Venusian documentation
.. <http://docs.repoze.org/venusian>`_.

完全な詳細については `Venusian documentation
<http://docs.repoze.org/venusian>`_ を読んでください。


.. Registering "Tweens"

.. _registering_tweens:

"tween" の登録
--------------------

.. .. note:: Tweens are a feature which were added in Pyramid 1.2.  They are
..    not available in any previous version.

.. note:: 

   tween は Pyramid 1.2 から追加された機能です。旧バージョンでは利用
   できません。


.. A :term:`tween` (a contraction of the word "between") is a bit of code that
.. sits between the Pyramid router's main request handling function and the
.. upstream WSGI component that uses :app:`Pyramid` as its "app".  This is a
.. feature that may be used by Pyramid framework extensions, to provide, for
.. example, Pyramid-specific view timing support bookkeeping code that examines
.. exceptions before they are returned to the upstream WSGI application.  Tweens
.. behave a bit like :term:`WSGI` middleware but they have the benefit of
.. running in a context in which they have access to the Pyramid
.. :term:`application registry` as well as the Pyramid rendering machinery.

:term:`tween` (単語 "between" の短縮形) は、 Pyramid ルーターのメイン
リクエスト処理関数と、 :app:`Pyramid` を "app" として使用する上流の WSGI
コンポーネントの間に存在するコードです。この機能は Pyramid フレームワーク
拡張によって使われることがあります。例えば、上流の WSGI アプリケーション
に返される前に例外を検証するような Pyramid 専用の view timing サポートの
ための bookkeeping コードを提供することができます。 tween は :term:`WSGI`
ミドルウェアと多少似た動作をしますが、 Pyramid のレンダリング機構と同様
Pyramid の :term:`application registry` にアクセスできるコンテキスト内で
動作するという利点があります。


.. Creating a Tween Factory

tween ファクトリの作成
~~~~~~~~~~~~~~~~~~~~~~~~

.. To make use of tweens, you must construct a "tween factory".  A tween factory
.. must be a globally importable callable which accepts two arguments:
.. ``handler`` and ``registry``.  ``handler`` will be the either the main
.. Pyramid request handling function or another tween.  ``registry`` will be the
.. Pyramid :term:`application registry` represented by this Configurator.  A
.. tween factory must return a tween when it is called.

tweens を利用するためには「tween ファクトリ」を構築する必要があります。
tween ファクトリは、 2 つの引数 ``handler`` と ``registry`` を受け取る
グローバルにインポート可能な callable です。 ``handler`` はメインの
Pyramid リクエスト処理関数か、あるいは別の tween のいずれかです。
``registry`` はこの Configurator によって表わされる Pyramid
:term:`application registry` です。 tween ファクトリは
呼び出された時に tween を返さなければなりません。


.. A tween is a callable which accepts a :term:`request` object and returns
.. a :term:`response` object.

tween は、 :term:`request` オブジェクトを受け取って、 :term:`response`
オブジェクトを返す callable です。


.. Here's an example of a tween factory:

これは tween ファクトリの例です:


.. code-block:: python
   :linenos:

    # in a module named myapp.tweens

    import time
    from pyramid.settings import asbool
    import logging

    log = logging.getLogger(__name__)

    def timing_tween_factory(handler, registry):
        if asbool(registry.settings.get('do_timing')):
            # if timing support is enabled, return a wrapper
            def timing_tween(request):
                start = time.time()
                try:
                    response = handler(request)
                finally:
                    end = time.time()
                    log.debug('The request took %s seconds' %
                              (end - start))
                return response
            return timing_tween
        # if timing support is not enabled, return the original
        # handler
        return handler


.. If you remember, a tween is an object which accepts a :term:`request` object
.. and which returns a :term:`response` argument.  The ``request`` argument to a
.. tween will be the request created by Pyramid's router when it receives a WSGI
.. request.  The response object will be generated by the downstream Pyramid
.. application and it should be returned by the tween.

覚えているかもしれませんが、 tween は :term:`request` オブジェクトを
受け取って :term:`response` を返すオブジェクトです。 tween に対する
``request`` 引数は、 Pyramid ルーターが WSGI リクエストを受け取った時
に生成されたリクエストです。レスポンスオブジェクトは下流の Pyramid
アプリケーションによって生成され、 tween によって返されます。


.. In the above example, the tween factory defines a ``timing_tween`` tween and
.. returns it if ``asbool(registry.settings.get('do_timing'))`` is true.  It
.. otherwise simply returns the handler it was given.  The ``registry.settings``
.. attribute is a handle to the deployment settings provided by the user
.. (usually in an ``.ini`` file).  In this case, if the user has defined a
.. ``do_timing`` setting, and that setting is ``True``, the user has said she
.. wants to do timing, so the tween factory returns the timing tween; it
.. otherwise just returns the handler it has been provided, preventing any
.. timing.

上記の例で tween ファクトリは ``timing_tween`` tween を定義していて、
``asbool(registry.settings.get('do_timing'))`` が true の場合はそれを
返します。そうでなければ渡されたハンドラを単に返します。
``registry.settings`` 属性は、ユーザによって提供される (通常 ``.ini``
ファイル中の) 設定へのハンドルです。この場合、ユーザが ``do_timing``
設定を定義していてその設定値が True なら、ユーザは timing を行いたいと
言ったことになり、したがって tween ファクトリは timing tween を返します;
そうでなければ timing は一切行わず、単に提供されたハンドラを返します。


.. The example timing tween simply records the start time, calls the downstream
.. handler, logs the number of seconds consumed by the downstream handler, and
.. returns the response.

この例 の timing tween は、単に開始時刻を記録し、下流のハンドラを呼び、
下流のハンドラによって消費された秒数を記録し、レスポンスを返します。


.. Registering an Implicit Tween Factory

暗黙的な tween ファクトリの登録
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Once you've created a tween factory, you can register it into the implicit
.. tween chain using the :meth:`pyramid.config.Configurator.add_tween` method
.. using its :term:`dotted Python name`.

tween ファクトリを作成したら、
:meth:`pyramid.config.Configurator.add_tween` メソッドを使用して、
その :term:`dotted Python name` を用いてそれを暗黙の tween チェインに
登録することができます。


.. Here's an example of registering the a tween factory as an "implicit"
.. tween in a Pyramid application:

これは、 Pyramid アプリケーションで tween ファクトリを「暗黙の」 tween
として登録する例です:


.. code-block:: python
   :linenos:

    from pyramid.config import Configurator
    config = Configurator()
    config.add_tween('myapp.tweens.timing_tween_factory')


.. Note that you must use a :term:`dotted Python name` as the first argument to
.. :meth:`pyramid.config.Configurator.add_tween`; this must point at a tween
.. factory.  You cannot pass the tween factory object itself to the method: it
.. must be :term:`dotted Python name` that points to a globally importable
.. object.  In the above example, we assume that a ``timing_tween_factory``
.. tween factory was defined in a module named ``myapp.tweens``, so the tween
.. factory is importable as ``myapp.tweens.timing_tween_factory``.

:meth:`pyramid.config.Configurator.add_tween` への最初の引数として
:term:`dotted Python name` を使用する必要があることに注意してください;
これは tween ファクトリを指していなければなりません。 tween ファクトリ
オブジェクト自体をメソッドへ渡すことはできません: それはグローバルに
インポート可能なオブジェクトを指す :term:`dotted Python name` である必要が
あります。上記の例では、 ``timing_tween_factory`` tween ファクトリが
``myapp.tweens`` という名前のモジュールに定義されていると仮定されます。
したがって、 tween ファクトリは ``myapp.tweens.timing_tween_factory``
のようにしてインポート可能です。


.. When you use :meth:`pyramid.config.Configurator.add_tween`, you're
.. instructing the system to use your tween factory at startup time unless the
.. user has provided an explicit tween list in his configuration.  This is
.. what's meant by an "implicit" tween.  A user can always elect to supply an
.. explicit tween list, reordering or disincluding implicitly added tweens.  See
.. :ref:`explicit_tween_ordering` for more information about explicit tween
.. ordering.

:meth:`pyramid.config.Configurator.add_tween` を使用する場合、
あなたはシステムに対して、設定で明示的な tween リストが提供されない限り
この tween ファクトリをスタートアップ時に使うように、と伝えています。
これが「暗黙の」 tween の意味することです。ユーザはいつでも、暗黙に追加
された tween を並び替えたり除外したりして、明示的な tween リストを提供
することを選択できます。 tween の明示的な順序についての詳細は
:ref:`explicit_tween_ordering` を参照してください。


.. If more than one call to :meth:`pyramid.config.Configurator.add_tween` is
.. made within a single application configuration, the tweens will be chained
.. together at application startup time.  The *first* tween factory added via
.. ``add_tween`` will be called with the Pyramid exception view tween factory as
.. its ``handler`` argument, then the tween factory added directly after that
.. one will be called with the result of the first tween factory as its
.. ``handler`` argument, and so on, ad infinitum until all tween factories have
.. been called. The Pyramid router will use the outermost tween produced by this
.. chain (the tween generated by the very last tween factory added) as its
.. request handler function.  For example:

1つのアプリケーション設定中で
:meth:`pyramid.config.Configurator.add_tween` への呼び出しが複数回
行われた場合、それらの tween はアプリケーションのスタートアップ時に
まとめてチェインされます。 ``add_tween`` によって追加された *最初の*
tween ファクトリが Pyramid 例外ビュー tween ファクトリをその
``handler`` 引数として呼び出され、次にその直後に追加された tween
ファクトリが最初の tween ファクトリの結果をその ``handler`` 引数として
呼び出されます。このようにして、すべての tween ファクトリが呼び出される
まで際限なく続きます。 Pyramid ルーターは、このチェインによって生成された
最も外側の tween (最後に追加された tween ファクトリによって生成された
tween) をそのリクエストハンドラ関数として使用します。例えば:


.. code-block:: python
   :linenos:

    from pyramid.config import Configurator

    config = Configurator()
    config.add_tween('myapp.tween_factory1')
    config.add_tween('myapp.tween_factory2')


.. The above example will generate an implicit tween chain that looks like
.. this:

上記の例は、以下に見るような暗黙の tween チェインを生成するでしょう:


::

    INGRESS (implicit)
    myapp.tween_factory2
    myapp.tween_factory1
    pyramid.tweens.excview_tween_factory (implicit)
    MAIN (implicit)


.. Suggesting Implicit Tween Ordering

暗黙的な tween 順序の指示
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. By default, as described above, the ordering of the chain is controlled
.. entirely by the relative ordering of calls to
.. :meth:`pyramid.config.Configurator.add_tween`.  However, the caller of
.. add_tween can provide an optional hint that can influence the implicit tween
.. chain ordering by supplying ``under`` or ``over`` (or both) arguments to
.. :meth:`~pyramid.config.Configurator.add_tween`.  These hints are only used
.. used when an explicit tween ordering is not used. See
.. :ref:`explicit_tween_ordering` for a description of how to set an explicit
.. tween ordering.

上述のように、デフォルトではチェインの順序は
:meth:`pyramid.config.Configurator.add_tween` の呼び出しの相対的な
順番によって完全にコントロールされます。しかし、 add_tween の呼び出し元は
:meth:`~pyramid.config.Configurator.add_tween` に ``under`` または
``over`` (あるいはその両方) の引数を渡すことで、暗黙の tween チェインの
順序に影響を及ぼすためのオプションのヒントを提供することができます。
これらのヒントは、明示的な tween 順序が使用されていない場合にだけ使用されます。
明示的な tween 順序を設定する方法の説明については、
:ref:`explicit_tween_ordering` を参照してください。


.. Allowable values for ``under`` or ``over`` (or both) are:

``under`` や ``over`` (またはその両方) に対して可能な値は:


.. - ``None`` (the default).

.. - A :term:`dotted Python name` to a tween factory: a string representing the
..   predicted dotted name of a tween factory added in a call to ``add_tween``
..   in the same configuration session.

.. - One of the constants :attr:`pyramid.tweens.MAIN`,
..   :attr:`pyramid.tweens.INGRESS`, or :attr:`pyramid.tweens.EXCVIEW`.

.. - An iterable of any combination of the above. This allows the user to specify
..   fallbacks if the desired tween is not included, as well as compatibility
..   with multiple other tweens.


- ``None`` (デフォルト)

- tween ファクトリに対する :term:`dotted Python name`: 同じ設定セッション
  内で ``add_tween`` 呼び出しで追加された tween ファクトリの predicted
  dotted name を表わす文字列。

- 定数 :attr:`pyramid.tweens.MAIN`, :attr:`pyramid.tweens.INGRESS` あるいは
  :attr:`pyramid.tweens.EXCVIEW` のうちの1つ。

- 上記のものの任意の組み合わせによる iterable 。これは、期待する tween
  が含まれていない場合や、他の複数の tween との互換性のために、ユーザが
  fallback を指定することを可能にします。


.. Effectively, ``under`` means "closer to the main Pyramid application than",
.. ``over`` means "closer to the request ingress than".

実質的に、 ``under`` は「メインの Pyramid アプリケーションにより近い」
ことを意味して、 ``over`` は「リクエストの入口により近い」ことを意味します。


.. For example, the following call to
.. :meth:`~pyramid.config.Configurator.add_tween` will attempt to place the
.. tween factory represented by ``myapp.tween_factory`` directly 'above' (in
.. ``ptweens`` order) the main Pyramid request handler.

例えば、以下の :meth:`~pyramid.config.Configurator.add_tween` に対する
呼び出しは、 ``myapp.tween_factory`` で表わされる tween ファクトリを
(``ptweens`` の順で) メインの Pyramid リクエストハンドラの直接「上」に
置こうとします。


.. code-block:: python
   :linenos:

   import pyramid.tweens

   config.add_tween('myapp.tween_factory', over=pyramid.tweens.MAIN)


.. The above example will generate an implicit tween chain that looks like
.. this:

上記の例は、以下に見るような暗黙の tween チェインを生成するでしょう:


::

    INGRESS (implicit)
    pyramid.tweens.excview_tween_factory (implicit)
    myapp.tween_factory
    MAIN (implicit)


.. Likewise, calling the following call to
.. :meth:`~pyramid.config.Configurator.add_tween` will attempt to place this
.. tween factory 'above' the main handler but 'below' a separately added tween
.. factory:

同様に、以下の :meth:`~pyramid.config.Configurator.add_tween` に対する
呼び出しでは、この tween ファクトリをメインハンドラより「上」で、別に追加
された tween ファクトリより「下」に置こうとします:


.. code-block:: python
   :linenos:

   import pyramid.tweens

   config.add_tween('myapp.tween_factory1',
                    over=pyramid.tweens.MAIN)
   config.add_tween('myapp.tween_factory2',
                    over=pyramid.tweens.MAIN,
                    under='myapp.tween_factory1')


.. The above example will generate an implicit tween chain that looks like
.. this:

上記の例は、以下に見るような暗黙の tween チェインを生成するでしょう:


::

    INGRESS (implicit)
    pyramid.tweens.excview_tween_factory (implicit)
    myapp.tween_factory1
    myapp.tween_factory2
    MAIN (implicit)


.. Specifying neither ``over`` nor ``under`` is equivalent to specifying
.. ``under=INGRESS``.

``over`` も ``under`` も指定しないことは、 ``under=INGRESS`` を指定した
ことと等価です。


.. If all options for ``under`` (or ``over``) cannot be found in the current
.. configuration, it is an error. If some options are specified purely for
.. compatibilty with other tweens, just add a fallback of MAIN or INGRESS.
.. For example, ``under=('someothertween', 'someothertween2', INGRESS)``.
.. This constraint will require the tween to be located under both the
.. 'someothertween' tween, the 'someothertween2' tween, and INGRESS. If any of
.. these is not in the current configuration, this constraint will only organize
.. itself based on the tweens that are present.

``under`` (あるいは ``over``) に対するすべてのオプションが現在の設定で
見つからない場合、それはエラーです。あるオプションが純粋に他の tween
との互換性のために指定される場合、単に MAIN または INGRESS の fallback
を追加してください。例えば、 ``under=('someothertween',
'someothertween2', INGRESS)`` 。この制約は、 tween を 'someothertween'
tween, 'someothertween2' tween, INGRESS のすべての下に置くことを要求
します。これらのうちのいずれかが現在の設定にない場合は、この制約は単に
存在する tween に基づいて自分自身を組織するでしょう。


.. Explicit Tween Ordering

.. _explicit_tween_ordering:

明示的な tween 順序
~~~~~~~~~~~~~~~~~~~~~~~

.. Implicit tween ordering is obviously only best-effort.  Pyramid will attempt
.. to provide an implicit order of tweens as best it can using hints provided by
.. calls to :meth:`~pyramid.config.Configurator.add_tween`, but because it's
.. only best-effort, if very precise tween ordering is required, the only
.. surefire way to get it is to use an explicit tween order.  The deploying user
.. can override the implicit tween inclusion and ordering implied by calls to
.. :meth:`~pyramid.config.Configurator.add_tween` entirely by using the
.. ``pyramid.tweens`` settings value.  When used, this settings value must be a
.. list of Python dotted names which will override the ordering (and inclusion)
.. of tween factories in the implicit tween chain.  For example:

暗黙の tween 順序は明らかに単なるベストエフォートです。 Pyramid は、
:meth:`~pyramid.config.Configurator.add_tween` の呼び出しから得られる
ヒントを用いてできるだけ暗黙の tween 順序を提供しようとしますが、
それは単なるベストエフォートなので、非常に正確な tween 順序が必要な場合、
それを得る唯一の方法は明示的な tween 順序を使用することです。
デプロイを行うユーザは、 ``pyramid.tweens`` 設定値を使用することで
:meth:`~pyramid.config.Configurator.add_tween` の呼び出しによって暗黙
的に指定された tween の選択および順序を完全にオーバーライドすることができます。
この設定値が使われた場合、それは暗黙の tween チェイン中での tween
ファクトリの順序 (または選択) をオーバーライドするような Python dotted names の
リストでなければなりません。例えば:


.. code-block:: ini
   :linenos:

   [app:main]
   use = egg:MyApp
   pyramid.reload_templates = true
   pyramid.debug_authorization = false
   pyramid.debug_notfound = false
   pyramid.debug_routematch = false
   pyramid.debug_templates = true
   pyramid.tweens = myapp.my_cool_tween_factory
                    pyramid.tweens.excview_tween_factory


.. In the above configuration, calls made during configuration to
.. :meth:`pyramid.config.Configurator.add_tween` are ignored, and the user is
.. telling the system to use the tween factories he has listed in the
.. ``pyramid.tweens`` configuration setting (each is a :term:`dotted Python
.. name` which points to a tween factory) instead of any tween factories added
.. via :meth:`pyramid.config.Configurator.add_tween`.  The *first* tween factory
.. in the ``pyramid.tweens`` list will be used as the producer of the effective
.. :app:`Pyramid` request handling function; it will wrap the tween factory
.. declared directly "below" it, ad infinitum.  The "main" Pyramid request
.. handler is implicit, and always "at the bottom".

上記の設定では、設定中に行われた
:meth:`pyramid.config.Configurator.add_tween` の呼び出しは無視されます。
また、ユーザは :meth:`pyramid.config.Configurator.add_tween` によって
追加された任意の tween ファクトリの代わりに ``pyramid.tweens`` 設定に
リストした tween ファクトリ (各々は tween ファクトリを指す
:term:`dotted Python name` です) を使用するようにシステムに指示しています。
``pyramid.tweens`` リスト中の *最初の* tween ファクトリは、
有効な :app:`Pyramid` リクエスト処理関数の producer として使用されるでしょう;
それは直下で宣言された tween ファクトリをラップして、それが
無制限に続きます。「メイン」 Pyramid リクエストハンドラは暗黙的です。
そして常に「一番下」にあります。


.. note::

   .. Pyramid's own :term:`exception view` handling logic is implemented
   .. as a tween factory function: :func:`pyramid.tweens.excview_tween_factory`.
   .. If Pyramid exception view handling is desired, and tween factories are
   .. specified via the ``pyramid.tweens`` configuration setting, the
   .. :func:`pyramid.tweens.excview_tween_factory` function must be added to the
   .. ``pyramid.tweens`` configuration setting list explicitly.  If it is not
   .. present, Pyramid will not perform exception view handling.

   Pyramid 自身の :term:`exception view` 処理ロジックは tween
   ファクトリ関数として実装されます:
   :func:`pyramid.tweens.excview_tween_factory` 。もし Pyramid 例外ビューの
   処理が行われることを期待していて、 ``pyramid.tweens`` 設定で
   tween ファクトリを指定しているなら、 ``pyramid.tweens`` 設定リストに
   明示的に :func:`pyramid.tweens.excview_tween_factory` 関数を追加
   しなければなりません。もしそれが存在しなければ、 Pyramid は例外ビュー
   処理を行ないません。


.. Tween Conflicts and Ordering Cycles

tween の衝突と順序の循環
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Pyramid will prevent the same tween factory from being added to the tween
.. chain more than once using configuration conflict detection.  If you wish to
.. add the same tween factory more than once in a configuration, you should
.. either: a) use a tween factory that is a separate globally importable
.. instance object from the factory that it conflicts with b) use a function or
.. class as a tween factory with the same logic as the other tween factory it
.. conflicts with but with a different ``__name__`` attribute or c) call
.. :meth:`pyramid.config.Configurator.commit` between calls to
.. :meth:`pyramid.config.Configurator.add_tween`.

Pyramid は、設定衝突検知を使用して同じ tween ファクトリが複数回
tween チェインに追加されることを防ぎます。設定に複数回同じ tween
ファクトリを追加したければ、以下のことをする必要があります: a) それが
衝突するファクトリとは別の、グローバルにインポート可能なインスタンス
オブジェクトである tween ファクトリを使用する b) tween ファクトリとして
それが衝突する別の tween ファクトリと同じロジックで異なる ``__name__``
属性を持つ関数またはクラスを使用する、あるいは c)
:meth:`pyramid.config.Configurator.add_tween` の複数回の呼び出しの間に
:meth:`pyramid.config.Configurator.commit` を呼び出す。


.. If a cycle is detected in implicit tween ordering when ``over`` and ``under``
.. are used in any call to "add_tween", an exception will be raised at startup
.. time.

"add_tween" への任意の呼び出しの中で ``over`` と ``under`` が使用されている
時に暗黙の tween 順序に循環が検知された場合、スタートアップ時に例外が発生
します。


.. Displaying Tween Ordering

tween 順序の表示
~~~~~~~~~~~~~~~~~~~~~~~~~

.. The ``ptweens`` command-line utility can be used to report the current
.. implict and explicit tween chains used by an application.  See
.. :ref:`displaying_tweens`.

``ptweens`` コマンドラインユーティリティを使って、アプリケーションによって
使用される現在の暗黙的および明示的な tween チェインを表示することができます。
:ref:`displaying_tweens` を参照してください。


.. _registering_thirdparty_predicates:

Adding A Third Party View, Route, or Subscriber Predicate
---------------------------------------------------------

.. note::

   Third-party view, route, and subscriber predicates are a feature new as of
   Pyramid 1.4.

.. _view_and_route_predicates:

View and Route Predicates
~~~~~~~~~~~~~~~~~~~~~~~~~

View and route predicates used during configuration allow you to narrow the
set of circumstances under which a view or route will match.  For example,
the ``request_method`` view predicate can be used to ensure a view callable
is only invoked when the request's method is ``POST``:


.. code-block:: python

    @view_config(request_method='POST')
    def someview(request):
        ...


Likewise, a similar predicate can be used as a *route* predicate:


.. code-block:: python

    config.add_route('name', '/foo', request_method='POST')


Many other built-in predicates exists (``request_param``, and others).  You
can add third-party predicates to the list of available predicates by using
one of :meth:`pyramid.config.Configurator.add_view_predicate` or
:meth:`pyramid.config.Configurator.add_route_predicate`.  The former adds a
view predicate, the latter a route predicate.

When using one of those APIs, you pass a *name* and a *factory* to add a
predicate during Pyramid's configuration stage.  For example:


.. code-block:: python

    config.add_view_predicate('content_type', ContentTypePredicate)


The above example adds a new predicate named ``content_type`` to the list of
available predicates for views.  This will allow the following view
configuration statement to work:


.. code-block:: python
   :linenos:

   @view_config(content_type='File')
   def aview(request): ...


The first argument to :meth:`pyramid.config.Configurator.add_view_predicate`,
the name, is a string representing the name that is expected to be passed to
``view_config`` (or its imperative analogue ``add_view``).

The second argument is a view or route predicate factory.  A view or route
predicate factory is most often a class with a constructor (``__init__``), a
``text`` method, a ``phash`` method and a ``__call__`` method.  For example:


.. code-block:: python
   :linenos:

    class ContentTypePredicate(object):
        def __init__(self, val, config):
            self.val = val

        def text(self):
            return 'content_type = %s' % (self.val,)

        phash = text

        def __call__(self, context, request):
            return getattr(context, 'content_type', None) == self.val


The constructor of a predicate factory takes two arguments: ``val`` and
``config``.  The ``val`` argument will be the argument passed to
``view_config`` (or ``add_view``).  In the example above, it will be the
string ``File``.  The second arg, ``config`` will be the Configurator
instance at the time of configuration.

The ``text`` method must return a string.  It should be useful to describe
the behavior of the predicate in error messages.

The ``phash`` method must return a string or a sequence of strings.  It's
most often the same as ``text``, as long as ``text`` uniquely describes the
predicate's name and the value passed to the constructor.  If ``text`` is
more general, or doesn't describe things that way, ``phash`` should return a
string with the name and the value serialized.  The result of ``phash`` is
not seen in output anywhere, it just informs the uniqueness constraints for
view configuration.

The ``__call__`` method of a predicate factory must accept a resource
(``context``) and a request, and must return ``True`` or ``False``.  It is
the "meat" of the predicate.

You can use the same predicate factory as both a view predicate and as a
route predicate, but you'll need to call ``add_view_predicate`` and
``add_route_predicate`` separately with the same factory.


.. _subscriber_predicates:

Subscriber Predicates
~~~~~~~~~~~~~~~~~~~~~

Subscriber predicates work almost exactly like view and route predicates.
They narrow the set of circumstances in which a subscriber will be called.
There are several minor differences between a subscriber predicate and a
view/route predicate:

- There are no default subscriber predicates.  You must register one to use
  one.

- The ``__call__`` method of a subscriber predicate accepts a single
  ``event`` object instead of a ``context`` and a ``request``.

- Not every subscriber predicate can be used with every event type.  Some
  subscriber predicates will assume a certain event type.

Here's an example of a subscriber predicate that can be used in conjunction
with a subscriber that subscribes to the :class:`pyramid.events.NewReqest`
event type.


.. code-block:: python
   :linenos:

    class RequestPathStartsWith(object):
        def __init__(self, val, config):
            self.val = val

        def text(self):
            return 'path_startswith = %s' % (self.val,)

        phash = text

        def __call__(self, event):
            return event.request.path.startswith(self.val)


Once you've created a subscriber predicate, it may registered via
:meth:`pyramid.config.Configurator.add_subscriber_predicate`.  For example:


.. code-block:: python

    config.add_subscriber_predicate(
        'request_path_startswith', RequestPathStartsWith)


Once a subscriber predicate is registered, you can use it in a call to
:meth:`pyramid.config.Configurator.add_subscriber` or to
:class:`pyramid.events.subscriber`.  Here's an example of using the
previously registered ``request_path_startswith`` predicate in a call to
:meth:`~pyramid.config.Configurator.add_subscriber`:


.. code-block:: python
   :linenos:

    # define a subscriber in your code

    def yosubscriber(event):
        event.request.yo = 'YO!'

    # and at configuration time

    config.add_subscriber(yosubscriber, NewRequest, 
           request_path_startswith='/add_yo')


Here's the same subscriber/predicate/event-type combination used via
:class:`~pyramid.events.subscriber`.


.. code-block:: python
   :linenos:

    from pyramid.events import subscriber

    @subscriber(NewRequest, request_path_startswith='/add_yo')
    def yosubscriber(event):
        event.request.yo = 'YO!'


In either of the above configurations, the ``yosubscriber`` callable will
only be called if the request path starts with ``/add_yo``.  Otherwise the
event subscriber will not be called.

Note that the ``request_path_startswith`` subscriber you defined can be used
with events that have a ``request`` attribute, but not ones that do not.  So,
for example, the predicate can be used with subscribers registered for
:class:`pyramid.events.NewRequest` and :class:`pyramid.events.ContextFound`
events, but it cannot be used with subscribers registered for
:class:`pyramid.events.ApplicationCreated` because the latter type of event
has no ``request`` attribute.  The point being: unlike route and view
predicates, not every type of subscriber predicate will necessarily be
applicable for use in every subscriber registration.  It is not the
responsibility of the predicate author to make every predicate make sense for
every event type; it is the responsibility of the predicate consumer to use
predicates that make sense for a particular event type registration.
