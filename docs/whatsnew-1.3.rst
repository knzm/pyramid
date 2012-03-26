What's New In Pyramid 1.3
=========================

.. This article explains the new features in :app:`Pyramid` version 1.3 as
.. compared to its predecessor, :app:`Pyramid` 1.2.  It also documents backwards
.. incompatibilities between the two versions and deprecations added to
.. :app:`Pyramid` 1.3, as well as software dependency changes and notable
.. documentation additions.

この記事は、 :app:`Pyramid` バージョン 1.3 とその前身である
:app:`Pyramid` 1.2 を比較して、新しい特徴について説明します。
さらに、2つのバージョンの間の後方非互換性と :app:`Pyramid` 1.3 に
加えられた廃止 (deprecation) 、ソフトウェア依存性の変更、
特に重要なドキュメンテーションの追加について文書化します。


.. Major Feature Additions

メジャー機能追加
-----------------------

.. The major feature additions in Pyramid 1.3 follow.

Pyramid 1.3 の主な機能追加は以下の通りです。


.. Python 3 Compatibility

Python 3 互換性
~~~~~~~~~~~~~~~~~~~~~~

.. image:: python-3.png

.. Pyramid continues to run on Python 2, but Pyramid is now also Python 3
.. compatible.  To use Pyramid under Python 3, Python 3.2 or better is required.

Pyramid は引き続き Python 2 上で動きますが、 Pyramid はさらに
Python 3 互換になりました。 Python 3 の下で Pyramid を使用するためには
Python 3.2 以上が要求されます。


.. Many Pyramid add-ons are already Python 3 compatible.  For example,
.. ``pyramid_debugtoolbar``, ``pyramid_jinja2``, ``pyramid_exclog``,
.. ``pyramid_tm``, ``pyramid_mailer``, and ``pyramid_handlers`` are all Python
.. 3-ready.  But other add-ons are known to work only under Python 2.  Also,
.. some scaffolding dependencies (particularly ZODB) do not yet work under
.. Python 3.

多くの Pyramid アドオンが既に Python 3 互換になっています。例えば、
``pyramid_debugtoolbar``, ``pyramid_jinja2``, ``pyramid_exclog``,
``pyramid_tm``, ``pyramid_mailer``, ``pyramid_handlers`` はすべて
Python 3 に対応しています。しかし、他のアドオンは Python 2 でのみ動く
ことが知られています。さらに、 scaffold が依存するいくつかのパッケージ
(特にZODB)は、まだ Python 3 の下で動きません。


.. Please be patient as we gain full ecosystem support for Python 3.  You can
.. see more details about ongoing porting efforts at
.. https://github.com/Pylons/pyramid/wiki/Python-3-Porting .

Python 3 に対する全面的なエコシステムのサポートを獲得するまでの間
しばらく待ってください。進行中の移植作業に関しては
https://github.com/Pylons/pyramid/wiki/Python-3-Porting
でより多くの詳細を見ることができます。


.. Python 3 compatibility required dropping some package dependencies and
.. support for older Python versions and platforms.  See the "Backwards
.. Incompatibilities" section below for more information.

Python 3 互換性のために、いくつかのパッケージ依存性の削除と、古い
Python バージョンおよびプラットフォームのサポートを打ち切ることが必要で
した。詳細は下記の "Backwards Incompatibilities" セクションを参照して
ください。


.. The ``paster`` Command Has Been Replaced

``paster`` コマンドが置き換えられました
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. We've replaced the ``paster`` command with Pyramid-specific analogues.  Why?
.. The libraries that supported the ``paster`` command named ``Paste`` and
.. ``PasteScript`` do not run under Python 3, and we were unwilling to port and
.. maintain them ourselves.  As a result, we've had to make some changes.

私たちは ``paster`` コマンドを Pyramid 固有の類似品に置き換えました。
なぜか? ``paster`` コマンドを提供する ``Paste`` と ``PasteScript``
という名前のライブラリが Python 3 の下では動かず、それらのライブラリを
私たち自身で移植してメンテナンスすることに気が進まなかったからです。
その結果、いくつかの変更を加える必要がありました。


.. Previously (in Pyramid 1.0, 1.1 and 1.2), you created a Pyramid application
.. using ``paster create``, like so:

以前 (Pyramid 1.0, 1.1 および 1.2) は、 ``paster create`` を使って
Pyramid アプリケーションを作成していました:


::

    $ myvenv/bin/paster create -t pyramid_starter foo


.. In 1.3, you're now instead required to create an application using
.. ``pcreate`` like so:

1.3 では、その代りに ``pcreate`` を使用してアプリケーションを作成する
必要があります:


::

    $ myvenv/bin/pcreate -s starter foo


.. ``pcreate`` is required to be used for internal Pyramid scaffolding;
.. externally distributed scaffolding may allow for both ``pcreate`` and/or
.. ``paster create``.

内蔵の Pyramid scaffold のためには ``pcreate`` を使う必要があります;
Pyramid の外部で配布された scaffold は、 ``pcreate`` と ``paster
create`` の両方またはどちらかに対応しています。


.. In previous Pyramid versions, you ran a Pyramid application like so:

Pyramid の前のバージョンでは、 Pyramid アプリケーションを起動するのに
以下のようにしていました:


::

    $ myvenv/bin/paster serve development.ini


.. Instead, you now must use the ``pserve`` command in 1.3:

この代わりに 1.3 では ``pserve`` コマンドを使用しなければなりません:


::

    $ myvenv/bin/pserve development.ini


.. The ``ini`` configuration file format supported by Pyramid has not changed.
.. As a result, Python 2-only users can install PasteScript manually and use
.. ``paster serve`` instead if they like.  However, using ``pserve`` will work
.. under both Python 2 and Python 3.

Pyramid がサポートする ``ini`` 設定ファイルのフォーマットは変わっていま
せん。そのため、 Python 2 のみを使っているユーザは、もしそうしたければ
PasteScript を手動でインストールして ``paster serve`` を代わりに使用す
ることができます。しかしながら、 ``pserve`` を使用すれば Python 2 と
Python 3 の両方の下で動くでしょう。


.. Analogues of ``paster pshell``, ``paster pviews``, ``paster request`` and
.. ``paster ptweens`` also exist under the respective console script names
.. ``pshell``, ``pviews``, ``prequest`` and ``ptweens``.

``paster pshell``, ``paster pviews``, ``paster request``, ``paster
ptweens`` にはそれぞれ、 ``pshell``, ``pviews``, ``prequest``,
``ptweens`` という対応するコンソールスクリプトが存在します。


.. ``paste.httpserver`` replaced by ``waitress`` in Scaffolds

scaffold 中で ``paste.httpserver`` は ``waitress`` で置き換えられました
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Because the ``paste.httpserver`` server we used previously in scaffolds is
.. not Python 3 compatible, we've made the default WSGI server used by Pyramid
.. scaffolding the :term:`waitress` server.  The waitress server is both Python
.. 2 and Python 3 compatible.

以前 scaffold の中で使用していた ``paste.httpserver`` サーバーが
Python 3 互換ではないので、私たちは Pyramid scaffold によって使用される
デフォルト WSGI サーバーを :term:`waitress` サーバーに変更しました。
waitress サーバーは Python 2 と Python 3 の両方と互換性があります。


.. Once you create a project from a scaffold, its ``development.ini`` and
.. ``production.ini`` will have the following line:

scaffold からプロジェクトを作成すれば ``development.ini`` と
``production.ini`` は次の行を持つでしょう:


::

    use = egg:waitress#main


.. Instead of this (which was the default in older versions):

これの代わりに (古いバージョンではこれがデフォルトでした):


::

    use = egg:Paste#http


.. note::

  .. ``paste.httpserver`` "helped" by converting header values that were Unicode
  .. into strings, which was a feature that subverted the :term:`WSGI`
  .. specification. The ``waitress`` server, on the other hand implements the
  .. WSGI spec more fully. This specifically may affect you if you are modifying
  .. headers on your responses. The following error might be an indicator of
  .. this problem: **AssertionError: Header values must be strings, please check
  .. the type of the header being returned.** A common case would be returning
  .. Unicode headers instead of string headers.

  ``paste.httpserver`` はヘッダーの値が Unicode だった場合 string 文字列
  に自動変換する機能がありました (それは WSGI 仕様に反した特徴でした)。
  それに対して ``waitress`` サーバーは WSGI 仕様をより完全に実装しています。
  これは特に、レスポンスのヘッダーを修正している場合に影響があるかもしれ
  ません。次のエラーがこの問題の指標になります: **AssertionError: Header
  values must be strings, please check the type of the header being
  returned.** よくあるケースは string ヘッダーの代わりに Unicode ヘッダーを
  返している場合です。


.. Compatibility Helper Library

互換性ヘルパーライブラリ
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. A new :mod:`pyramid.compat` module was added which provides Python 2/3
.. straddling support for Pyramid add-ons and development environments.

新しく :mod:`pyramid.compat` モジュールが加えられました。
このモジュールは Pyramid アドオンおよび開発環境に対して
Python 2/3 にまたがるサポートを提供します。


Introspection
~~~~~~~~~~~~~

.. A configuration introspection system was added; see
.. :ref:`using_introspection` and :ref:`introspection` for more information on
.. using the introspection system as a developer.

設定情報の introspection システムが加えられました; 開発者として
introspection システムを使用することについての詳細は、
:ref:`using_introspection` と :ref:`introspection` を参照してください。


.. The latest release of the pyramid debug toolbar (0.9.7+) provides an
.. "Introspection" panel that exposes introspection information to a Pyramid
.. application developer.

最新の Pyramid デバッグツールバー (0.9.7+) は、 Pyramid アプリケーション
開発者に introspection 情報を露出する "Introspection" パネルを提供します。


.. New APIs were added to support introspection
.. :attr:`pyramid.registry.Introspectable`,
.. :attr:`pyramid.config.Configurator.introspector`,
.. :attr:`pyramid.config.Configurator.introspectable`,
.. :attr:`pyramid.registry.Registry.introspector`.

introspection をサポートするために新しい API が追加されました:
:attr:`pyramid.registry.Introspectable`,
:attr:`pyramid.config.Configurator.introspector`,
:attr:`pyramid.config.Configurator.introspectable`,
:attr:`pyramid.registry.Registry.introspector`


.. ``@view_defaults`` Decorator

``@view_defaults`` デコレータ
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. If you use a class as a view, you can use the new
.. :class:`pyramid.view.view_defaults` class decorator on the class to provide
.. defaults to the view configuration information used by every ``@view_config``
.. decorator that decorates a method of that class.

クラスをビューとして使用する場合、そのクラスに対して新しい
:class:`pyramid.view.view_defaults` クラスデコレータを使用することがで
きます。このクラスデコレータを使うと、そのクラスのメソッドをデコレート
するすべての ``@view_config`` デコレータによって使用されるビュー設定情報
にデフォルトを提供することができます。


.. For instance, if you've got a class that has methods that represent "REST
.. actions", all which are mapped to the same route, but different request
.. methods, instead of this:

例えば、 "REST アクション" を表わすメソッドを持つクラスがある場合、すな
わち、すべてのメソッドが同じルートにマップされリクエストメソッドだけが
異なる場合、このようにする代わりに:


.. code-block:: python
   :linenos:

   from pyramid.view import view_config
   from pyramid.response import Response

   class RESTView(object):
       def __init__(self, request):
           self.request = request

       @view_config(route_name='rest', request_method='GET')
       def get(self):
           return Response('get')

       @view_config(route_name='rest', request_method='POST')
       def post(self):
           return Response('post')

       @view_config(route_name='rest', request_method='DELETE')
       def delete(self):
           return Response('delete')


.. You can do this:

こうすることができます:


.. code-block:: python
   :linenos:

   from pyramid.view import view_defaults
   from pyramid.view import view_config
   from pyramid.response import Response

   @view_defaults(route_name='rest')
   class RESTView(object):
       def __init__(self, request):
           self.request = request

       @view_config(request_method='GET')
       def get(self):
           return Response('get')

       @view_config(request_method='POST')
       def post(self):
           return Response('post')

       @view_config(request_method='DELETE')
       def delete(self):
           return Response('delete')


.. This also works for imperative view configurations that involve a class.

これは、クラスに作用する命令的なビュー設定でも同様に機能します。


.. See :ref:`view_defaults` for more information.

詳細は :ref:`view_defaults` を参照してください。


.. Extending a Request without Subclassing

サブクラス化をしない Request 拡張
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. It is now possible to extend a :class:`pyramid.request.Request` object
.. with property descriptors without having to create a custom request factory.
.. The new method :meth:`pyramid.config.Configurator.set_request_property`
.. provides an entry point for addons to register properties which will be
.. added to each request. New properties may be reified, effectively caching
.. the return value for the lifetime of the instance. Common use-cases for this
.. would be to get a database connection for the request or identify the current
.. user. The new method :meth:`pyramid.request.Request.set_property` has been
.. added, as well, but the configurator method should be preferred as it
.. provides conflict detection and consistency in the lifetime of the
.. properties.

プロパティディスクリプタを持つ :class:`pyramid.request.Request`
オブジェクトを、カスタムリクエストファクトリを作ることなく拡張すること
ができるようになりました。新しいメソッド
:meth:`pyramid.config.Configurator.set_request_property` は、アドオンに
対し各リクエストに加えられるプロパティを登録するためのエントリポイント
を提供します。新しいプロパティは、インスタンスのライフタイムにおいて
返り値を事実上キャッシュして、具象化されるかもしれません。このための
通常のユースケースは、リクエストのためにデータベース接続を得たり、
カレントユーザーを識別したりすることです。新メソッド
:meth:`pyramid.request.Request.set_property` も追加されましたが、
configurator メソッドの方が、プロパティのライフタイムにおける
矛盾検知および一貫性を提供するので推奨されます。


.. Not Found and Forbidden View Helpers

Not Found と Forbidden ビューヘルパー
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Not Found helpers:

Not Found ヘルパー:


.. - New API: :meth:`pyramid.config.Configurator.add_notfound_view`.  This is a
..   wrapper for :meth:`pyramid.Config.configurator.add_view` which provides
..   support for an "append_slash" feature as well as doing the right thing when
..   it comes to permissions (a not found view should always be public).  It
..   should be preferred over calling ``add_view`` directly with
..   ``context=HTTPNotFound`` as was previously recommended.

- 新 API: :meth:`pyramid.config.Configurator.add_notfound_view` 。
  これは :meth:`pyramid.Config.configurator.add_view` のラッパーで、
  "append_slash" 機能のサポートを提供すると同時にパーミッションに関して
  正しいことを行います (not found ビューは常に public であるべきです)。
  以前に推奨されていた ``context=HTTPNotFound`` で ``add_view`` を直接
  呼び出すよりも、こちらの方がより好まれます。


.. - New API: :class:`pyramid.view.notfound_view_config`.  This is a decorator
..   constructor like :class:`pyramid.view.view_config` that calls
..   :meth:`pyramid.config.Configurator.add_notfound_view` when scanned.  It
..   should be preferred over using ``pyramid.view.view_config`` with
..   ``context=HTTPNotFound`` as was previously recommended.

- 新 API: :class:`pyramid.view.notfound_view_config` 。
  これは :class:`pyramid.view.view_config` のようなデコレータコンストラクタで、
  スキャンされた時に :meth:`pyramid.config.Configurator.add_notfound_view`
  を呼び出します。以前に推奨されていた ``context=HTTPNotFound`` で
  ``pyramid.view.view_config`` を使用するよりも、こちらの方がより好まれます。


.. Forbidden helpers:

Forbidden ヘルパー:


.. - New API: :meth:`pyramid.config.Configurator.add_forbidden_view`.  This is a
..   wrapper for :meth:`pyramid.Config.configurator.add_view` which does the
..   right thing about permissions.  It should be preferred over calling
..   ``add_view`` directly with ``context=HTTPForbidden`` as was previously
..   recommended.

- 新 API: :meth:`pyramid.config.Configurator.add_forbidden_view` 。
  これは :meth:`pyramid.Config.configurator.add_view` のラッパーで、
  パーミッションに関して正しいことを行います。
  以前に推奨されていた ``context=HTTPForbidden`` で ``add_view`` を直接
  呼び出すよりも、こちらの方がより好まれます。


.. - New API: :class:`pyramid.view.forbidden_view_config`.  This is a decorator
..   constructor like :class:`pyramid.view.view_config` that calls
..   :meth:`pyramid.config.Configurator.add_forbidden_view` when scanned.  It
..   should be preferred over using ``pyramid.view.view_config`` with
..   ``context=HTTPForbidden`` as was previously recommended.

- 新 API: :class:`pyramid.view.forbidden_view_config` 。
  これは :class:`pyramid.view.view_config` のようなデコレータコンストラクタで、
  スキャンされた時に :meth:`pyramid.config.Configurator.add_forbidden_view`
  を呼び出します。以前に推奨されていた ``context=HTTPForbidden`` で
  ``pyramid.view.view_config`` を使用するよりも、こちらの方がより好まれます。


.. Minor Feature Additions

マイナー機能追加
-----------------------

.. - New APIs: :class:`pyramid.path.AssetResolver` and
..   :class:`pyramid.path.DottedNameResolver`.  The former can be used to
..   resolve an :term:`asset specification` to an API that can be used to read
..   the asset's data, the latter can be used to resolve a :term:`dotted Python
..   name` to a module or a package.

- 新 API: :class:`pyramid.path.AssetResolver` と
  :class:`pyramid.path.DottedNameResolver` 。前者は :term:`asset
  specification` から asset のデータを読むために使用することのできる
  API へと解決するために使用されます。後者は :term:`dotted Python
  name` からモジュールまたはパッケージへと解決するために使用されます。


.. - A ``mako.directories`` setting is no longer required to use Mako templates
..   Rationale: Mako template renderers can be specified using an absolute asset
..   spec.  An entire application can be written with such asset specs,
..   requiring no ordered lookup path.

- Mako テンプレートを使用するために ``mako.directories`` 設定は必須で
  なくなりました。根拠: Mako テンプレートレンダラーは絶対 asset spec を
  使用して指定することができます。アプリケーション全体を、順序付きのルッ
  クアップパスを要求せずに、そのような asset spec で書くことができます。


.. - ``bpython`` interpreter compatibility in ``pshell``.  See
..   :ref:`ipython_or_bpython` for more information.

- ``pshell`` における ``bpython`` インタープリター互換性。詳細は
  :ref:`ipython_or_bpython` を参照してください。


.. - Added :func:`pyramid.paster.get_appsettings` API function.  This function
..   returns the settings defined within an ``[app:...]`` section in a
..   PasteDeploy ``ini`` file.

- :func:`pyramid.paster.get_appsettings` API 関数が追加されました。
  この関数は、 PasteDeploy ``ini`` ファイルの ``[app:...]`` セクション内に
  定義された設定を返します。


.. - Added :func:`pyramid.paster.setup_logging` API function.  This function
..   sets up Python logging according to the logging configuration in a
..   PasteDeploy ``ini`` file.

- :func:`pyramid.paster.setup_logging` API 関数が追加されました。
  この関数は、 PasteDeploy ``ini`` ファイル中の logging 設定に従い
  Python logging をセットアップします。


.. - Configuration conflict reporting is reported in a more understandable way
..   ("Line 11 in file..." vs. a repr of a tuple of similar info).

- 設定衝突レポートは、より理解しやすい方法で報告されます。
  ("Line 11 in file..." vs. 同じ情報のタプルの repr)


.. - We allow extra keyword arguments to be passed to the
..   :meth:`pyramid.config.Configurator.action` method.

- :meth:`pyramid.config.Configurator.action` メソッドに余分なキーワード
  引数を渡すことができるようになりました。


.. - Responses generated by Pyramid's :class:`pyramid.views.static_view` now use
..   a ``wsgi.file_wrapper`` (see
..   http://www.python.org/dev/peps/pep-0333/#optional-platform-specific-file-handling)
..   when one is provided by the web server.

- Pyramid の :class:`pyramid.views.static_view` によって生成された
  レスポンスは、ウェブサーバが ``wsgi.file_wrapper``
  (http://www.python.org/dev/peps/pep-0333/#optional-platform-specific-file-handling を参照)
  を提供する場合にはそれを使用するようになりました。


.. - The :meth:`pyramid.config.Configurator.scan` method can be passed an
..   ``ignore`` argument, which can be a string, a callable, or a list
..   consisting of strings and/or callables.  This feature allows submodules,
..   subpackages, and global objects from being scanned.  See
..   http://readthedocs.org/docs/venusian/en/latest/#ignore-scan-argument for
..   more information about how to use the ``ignore`` argument to ``scan``.

- :meth:`pyramid.config.Configurator.scan` メソッドは ``ignore`` 引数
  を渡すことができます。それは、文字列、 callable 、または文字列と
  callable からなるリストです。この機能は、スキャンからサブモジュール、
  サブパッケージおよびグローバルなオブジェクトを許可します。
  スキャンに ``ignore`` 引数を使用する方法についての詳細は、
  http://readthedocs.org/docs/venusian/en/latest/#ignore-scan-argument
  を参照してください。


.. - Add :meth:`pyramid.config.Configurator.add_traverser` API method.  See
..   :ref:`changing_the_traverser` for more information.  This is not a new
..   feature, it just provides an API for adding a traverser without needing to
..   use the ZCA API.

- :meth:`pyramid.config.Configurator.add_travers` API メソッドが追加され
  ました。詳細は :ref:`changing_the_traverser` を参照してください。
  これは新機能ではありません。単に ZCA API を使用する必要なしにトラバーサー
  を追加するための API を提供します。


.. - Add :meth:`pyramid.config.Configurator.add_resource_url_adapter` API
..   method.  See :ref:`changing_resource_url` for more information.  This is
..   not a new feature, it just provides an API for adding a resource url
..   adapter without needing to use the ZCA API.

- :meth:`pyramid.config.Configurator.add_resource_url_adapter` API
  メソッドが追加されました。詳細は :ref:`changing_resource_url` を参照
  してください。これは新機能ではありません。単に ZCA APIを使用する必要
  なしにリソース URL アダプターを追加するための API を提供します。


.. - The :meth:`pyramid.config.Configurator.scan` method can now be passed an
..   ``ignore`` argument, which can be a string, a callable, or a list
..   consisting of strings and/or callables.  This feature allows submodules,
..   subpackages, and global objects from being scanned.  See
..   http://readthedocs.org/docs/venusian/en/latest/#ignore-scan-argument for
..   more information about how to use the ``ignore`` argument to ``scan``.

- :meth:`pyramid.config.Configurator.scan` メソッドは ``ignore`` 引数
  を渡すことができるようになりました。それは、文字列、 callable 、あるいは
  文字列と callable からなるリストです。この機能は、スキャンからサブ
  モジュール、サブパッケージおよびグローバルなオブジェクトを許可します。
  スキャンに ``ignore`` 引数を使用する方法についての詳細は、
  http://readthedocs.org/docs/venusian/en/latest/#ignore-scan-argument
  を参照してください。
  (訳注: この段落は3つ前の段落と重複してる?)


.. - Better error messages when a view callable returns a value that cannot be
..   converted to a response (for example, when a view callable returns a
..   dictionary without a renderer defined, or doesn't return any value at all).
..   The error message now contains information about the view callable itself
..   as well as the result of calling it.

- ビュー callable がレスポンスに変換できない値を返したとき
  (例えば、ビュー callable がレンダラー定義なしで辞書を返した場合、ある
  いは値を全く返さない場合) のより良いエラーメッセージ。エラーメッセージ
  には、ビュー callable 自身に関する情報と、それを呼んだ結果が含まれる
  ようになりました。


.. - Better error message when a .pyc-only module is ``config.include`` -ed.
..   This is not permitted due to error reporting requirements, and a better
..   error message is shown when it is attempted.  Previously it would fail with
..   something like "AttributeError: 'NoneType' object has no attribute
..   'rfind'".

- .pyc のみのモジュールが ``config.include`` された場合のより良いエラー
  メッセージ。これはエラー報告の必要条件により許可されず、それが試みら
  れた場合より良いエラーメッセージが示されます。以前は、以下のような
  エラーが出て失敗していました: "AttributeError: 'NoneType' object has
  no attribute 'rfind'"


.. - The system value ``req`` is now supplied to renderers as an alias for
..   ``request``.  This means that you can now, for example, in a template, do
..   ``req.route_url(...)`` instead of ``request.route_url(...)``.  This is
..   purely a change to reduce the amount of typing required to use request
..   methods and attributes from within templates.  The value ``request`` is
..   still available too, this is just an alternative.

- システム値 ``req`` が ``request`` の別名としてレンダラーに提供される
  ようになりました。これは、例えばテンプレートの中で、
  ``request.route_url(...)`` の代わりに ``req.route_url(...)`` のように
  できるということです。これは純粋にテンプレートに内部でリクエストメソッド
  と属性を使用するのに必要なタイピング量を減らすための変更です。
  値 ``request`` もまだ利用可能です。これは単に別の選択肢です。


.. - A new interface was added: :class:`pyramid.interfaces.IResourceURL`.  An
..   adapter implementing its interface can be used to override resource URL
..   generation when :meth:`pyramid.request.Request.resource_url` is called.
..   This interface replaces the now-deprecated
..   ``pyramid.interfaces.IContextURL`` interface.

- 新しいインターフェースが追加されました:
  :class:`pyramid.interfaces.IResourceURL` 。このインターフェースを
  実装するアダプターは、
  :meth:`pyramid.request.Request.resource_url` が呼ばれる際にリソース
  URL 生成を無視するために使用することができます。
  このインターフェースは今では廃止された
  ``pyramid.interfaces.IContextURL`` インターフェースを置き換えます。


.. - The dictionary passed to a resource's ``__resource_url__`` method (see
..   :ref:`overriding_resource_url_generation`) now contains an ``app_url`` key,
..   representing the application URL generated during
..   :meth:`pyramid.request.Request.resource_url`.  It represents a potentially
..   customized URL prefix, containing potentially custom scheme, host and port
..   information passed by the user to ``request.resource_url``.  It should be
..   used instead of ``request.application_url`` where necessary.

- リソースの ``__resource_url__`` メソッドに渡された辞書
  (:ref:`overriding_resource_url_generation` を参照) は、
  :meth:`pyramid.request.Request.resource_url` で生成されたアプリケーション
  URL を表わす ``app_url`` キーを含むようになりました。それは、潜在的
  にカスタマイズされた URL プレフィックスを表わします。ユーザによって
  ``request.resource_url`` に渡された潜在的なカスタムスキーム、ホスト
  およびポート情報が含まれます。必要なところでは、
  ``request.application_url`` の代わりに使用されるべきです。


.. - The :meth:`pyramid.request.Request.resource_url` API now accepts these
..   arguments: ``app_url``, ``scheme``, ``host``, and ``port``.  The app_url
..   argument can be used to replace the URL prefix wholesale during url
..   generation.  The ``scheme``, ``host``, and ``port`` arguments can be used
..   to replace the respective default values of ``request.application_url``
..   partially.

- :meth:`pyramid.request.Request.resource_url` API はこれらの引数を
  受け取るようになりました: ``app_url``, ``scheme``, ``host``, ``port`` 。
  ``app_url`` 引数は URL 生成の際に大規模に URL プリフィックスを置き換える
  ために使用することができます。 ``scheme``, ``host``, ``port`` 引数は
  ``request.application_url`` のそれぞれのデフォルト値を部分的に置き換える
  ために使用できます。


.. - A new API named :meth:`pyramid.request.Request.resource_path` now exists.
..   It works like :meth:`pyramid.request.Request.resource_url` but produces a
..   relative URL rather than an absolute one.

- :meth:`pyramid.request.Request.resource_path` という名前の新しい API
  が存在するようになりました。これは
  :meth:`pyramid.request.Request.resource_url` のように作動しますが、
  絶対的ではなく相対的な URL を生成します。


.. - The :meth:`pyramid.request.Request.route_url` API now accepts these
..   arguments: ``_app_url``, ``_scheme``, ``_host``, and ``_port``.  The
..   ``_app_url`` argument can be used to replace the URL prefix wholesale
..   during url generation.  The ``_scheme``, ``_host``, and ``_port`` arguments
..   can be used to replace the respective default values of
..   ``request.application_url`` partially.

- :meth:`pyramid.request.Request.route_url` API は、これらの引数を
  受け取るようになりました: ``_app_url``, ``_scheme``, ``_host``, ``_port`` 。
  ``_app_url`` 引数は URL 生成の際に大規模に URL プリフィックスを置き換える
  ために使用することができます。 ``_scheme``, ``_host``, ``_port`` 引数は
  ``request.application_url`` のそれぞれのデフォルト値を部分的に置き換える
  ために使用できます。


.. - New APIs: :class:`pyramid.response.FileResponse` and
..   :class:`pyramid.response.FileIter`, for usage in views that must serve
..   files "manually".

- 新しいAPI: :class:`pyramid.response.FileResponse` および
  :class:`pyramid.response.FileIter` 。これらはファイルを「手動で」
  返す必要のあるビューで使用するためのものです。


.. Backwards Incompatibilities

後方非互換性
---------------------------

.. - Pyramid no longer runs on Python 2.5.  This includes the most recent
..   release of Jython and the Python 2.5 version of Google App Engine.

..   The reason?  We could not easily "straddle" Python 2 and 3 versions and
..   support Python 2 versions older than Python 2.6.  You will need Python 2.6
..   or better to run this version of Pyramid.  If you need to use Python 2.5,
..   you should use the most recent 1.2.X release of Pyramid.

- Pyramid はもう Python 2.5 上で動きません。これは Jython の最新の
  リリースおよび Google App Engine の Python 2.5 バージョンを含みます。

  理由?  Python 2 と 3 の複数のバージョンに「またがって (straggle)」かつ
  Python 2.6 以前の古い Python 2 バージョンをサポートすることは容易では
  ありません。Pyramid のこのバージョンを実行するには Python 2.6 以上が
  必要です。もし Python 2.5 を使用する必要があれば、 Pyramid 1.2.X の
  最新のリリースを使用してください。


.. - The names of available scaffolds have changed and the flags supported by
..   ``pcreate`` are different than those that were supported by ``paster
..   create``.  For example, ``pyramid_alchemy`` is now just ``alchemy``.

- 利用できる scaffold の名前が変わりました。また、 ``pcreate`` がサポート
  するフラグは ``paster create`` のサポートするフラグとは異なります。
  例えば ``pyramid_alchemy`` は単に ``alchemy`` になりました。


.. - The ``paster`` command is no longer the documented way to create projects,
..   start the server, or run debugging commands.  To create projects from
..   scaffolds, ``paster create`` is replaced by the ``pcreate`` console script.
..   To serve up a project, ``paster serve`` is replaced by the ``pserve``
..   console script.  New console scripts named ``pshell``, ``pviews``,
..   ``proutes``, and ``ptweens`` do what their ``paster <commandname>``
..   equivalents used to do.  All relevant narrative documentation has been
..   updated.  Rationale: the Paste and PasteScript packages do not run under
..   Python 3.

- ``paster`` コマンドは、プロジェクトを作成したり、サーバーを始めたり、
  デバッグコマンドを実行したりするための文書化された方法ではなくなりま
  した。 scaffold からプロジェクトを作成するのに、 ``paster create`` は
  ``pcreate`` コンソールスクリプトに置き換えられます。プロジェクトを
  実行するのに、 ``paster serve`` は ``pserve`` コンソールスクリプト
  に置き換えられます。 ``pshell``, ``pviews``, ``proutes``, ``ptweens``
  という名前の新しいコンソールスクリプトは、それらの
  ``paster <コマンド名>`` 等価物が行っていたことを行います。
  関連する narrative documentation がすべて更新されました。根拠:
  Paste と PasteScript パッケージは Python 3 の下で動きません。


.. - The default WSGI server run as the result of ``pserve`` from newly rendered
..   scaffolding is now the ``waitress`` WSGI server instead of the
..   ``paste.httpserver`` server.  Rationale: the Paste and PasteScript packages
..   do not run under Python 3.

- 新しく生成された scaffold で ``pserve`` を実行した場合、デフォルトの
  WSGI サーバーは ``paste.httpserver`` サーバーの代わりに ``waitress``
  WSGI サーバーになりました。根拠: Paste と PasteScript パッケージは
  Python 3 の下で動きません。


.. - The ``pshell`` command (see "paster pshell") no longer accepts a
..   ``--disable-ipython`` command-line argument.  Instead, it accepts a ``-p``
..   or ``--python-shell`` argument, which can be any of the values ``python``,
..   ``ipython`` or ``bpython``.

- ``pshell`` コマンド ("paster pshell" を参照) はコマンドライン引数
  ``--disable-ipython`` を受け付けなくなりました。代わりに ``-p``
  引数または ``--python-shell`` 引数を受け付けます。その値は ``python``,
  ``ipython`` or ``bpython`` のいずれかです。


.. - Removed the ``pyramid.renderers.renderer_from_name`` function.  It has been
..   deprecated since Pyramid 1.0, and was never an API.

- ``pyramid.renderers.renderer_from_name`` 関数が削除されました。それは
  Pyramid 1.0 以降廃止されており、 API ではありませんでした。


.. - To use ZCML with versions of Pyramid >= 1.3, you will need ``pyramid_zcml``
..   version >= 0.8 and ``zope.configuration`` version >= 3.8.0.  The
..   ``pyramid_zcml`` package version 0.8 is backwards compatible all the way to
..   Pyramid 1.0, so you won't be warned if you have older versions installed
..   and upgrade Pyramid itself "in-place"; it may simply break instead
..   (particularly if you use ZCML's ``includeOverrides`` directive).

- Pyramid >= 1.3 バージョンと共に ZCML を使用するために、
  ``pyramid_zcml`` バージョン >= 0.8 と ``zope.configuration`` バージョン
  >= 3.8.0 が必要です。 ``pyramid_zcml`` パッケージのバージョン 0.8
  は Pyramid 1.0 までずっと後方互換性を持ちます。したがって、より古いバー
  ジョンをインストールしていて Pyramid 自体を "in-place" でアップグレード
  した場合、警告されません; その代わりに単に壊れるでしょう。
  (特に ZCML の ``includeOverrides`` ディレクティブを使用している場合)


.. - String values passed to :meth:`Pyramid.request.Request.route_url` or
..   :meth:`Pyramid.request.Request.route_path` that are meant to replace
..   "remainder" matches will now be URL-quoted except for embedded slashes. For
..   example:

..      config.add_route('remain', '/foo*remainder')
..      request.route_path('remain', remainder='abc / def')
..      # -> '/foo/abc%20/%20def'

..   Previously string values passed as remainder replacements were tacked on
..   untouched, without any URL-quoting.  But this doesn't really work logically
..   if the value passed is Unicode (raw unicode cannot be placed in a URL or in
..   a path) and it is inconsistent with the rest of the URL generation
..   machinery if the value is a string (it won't be quoted unless by the
..   caller).

..   Some folks will have been relying on the older behavior to tack on query
..   string elements and anchor portions of the URL; sorry, you'll need to
..   change your code to use the ``_query`` and/or ``_anchor`` arguments to
..   ``route_path`` or ``route_url`` to do this now.

- :meth:`Pyramid.request.Request.route_url` または
  :meth:`Pyramid.request.Request.route_path` に渡された "remainder"
  マッチを置き換えることを意図した文字列の値は、埋め込まれたスラッシュ
  を除いて URL クォートされるようになりました。例えば::

     config.add_route('remain', '/foo*remainder')
     request.route_path('remain', remainder='abc / def')
     # -> '/foo/abc%20/%20def'

  以前は、 remainder 置換として渡された文字列の値は URLクォートされる
  ことなくそのまま扱われていました。しかし、渡された値が Unicode である
  場合、これは実際のところ理論的に動きません(生の Unicode は URL または
  パスに含めることができません)。また、値が文字列である場合、それは他の
  URL 生成機構と一致しません(呼び出し元でしなければ、 URL クォートされ
  ません)


.. - If you pass a bytestring that contains non-ASCII characters to
..   :meth:`pyramid.config.Configurator.add_route` as a pattern, it will now
..   fail at startup time.  Use Unicode instead.

- 非 ASCII 文字を含むバイト文字列をパターンとして
  :meth:`pyramid.config.Configurator.add_route` に渡した場合、
  スタートアップ時に失敗します。 Unicode を代わりに使用してください。


.. - The ``path_info`` route and view predicates now match against
..   ``request.upath_info`` (Unicode) rather than ``request.path_info``
..   (indeterminate value based on Python 3 vs. Python 2).  This has to be done
..   to normalize matching on Python 2 and Python 3.

- ``path_info`` ルートとビュー述語は、 ``request.path_info`` (Python 3
  と Python 2 で不定の値) ではなく ``request.upath_info`` (Unicode)
  に対してマッチするようになりました。 Python 2 と Python 3 でマッチを
  標準化するために、これを行わなければなりませんでした。


.. - The ``match_param`` view predicate no longer accepts a dict. This will have
..   no negative affect because the implementation was broken for dict-based
..   arguments.

- ``match_param`` ビュー述語は dict を受け付けなくなりました。
  これによるネガティブな影響はないでしょう。
  なぜなら dict ベースの引数に対する実装は壊れていたからです。


.. - The ``pyramid.interfaces.IContextURL`` interface has been deprecated.
..   People have been instructed to use this to register a resource url adapter
..   in the "Hooks" chapter to use to influence
..   :meth:`pyramid.request.Request.resource_url` URL generation for resources
..   found via custom traversers since Pyramid 1.0.

..   The interface still exists and registering an adapter using it as
..   documented in older versions still works, but this interface will be
..   removed from the software after a few major Pyramid releases.  You should
..   replace it with an equivalent :class:`pyramid.interfaces.IResourceURL`
..   adapter, registered using the new
..   :meth:`pyramid.config.Configurator.add_resource_url_adapter` API.  A
..   deprecation warning is now emitted when a
..   ``pyramid.interfaces.IContextURL`` adapter is found when
..   :meth:`pyramid.request.Request.resource_url` is called.

- ``pyramid.interfaces.IContextURL`` インターフェースが廃止されました。
  Pyramid 1.0 以降、カスタムトラバーサーによって見つかったリソースに対する
  :meth:`pyramid.request.Request.resource_url` URL 生成に影響を及ぼすために
  リソース URL アダプターを登録するのにこれを使用するように "Hooks" 章の中で
  指示されていました。

  このインターフェースはまだ存在します。また、古いバージョンで文書化
  されていたようにそれを使用してアダプターを登録することはまだ動きます。
  しかし、このインターフェースは Pyramid のいくつかのメジャーリリース後
  にソフトウェアから除去されるでしょう。新しい
  :meth:`pyramid.config.Configurator.add_resource_url_adapter` APIを使用
  して登録された等価な :meth:`pyramid.interfaces.IResourceURL` アダプター
  に置き換えてください。
  :meth:`pyramid.request.Request.resource_url` が呼ばれたときに
  ``pyramid.interfaces.IContextURL`` アダプターが見つかった場合、
  deprecation 警告が発生します。


.. - Remove ``pyramid.config.Configurator.with_context`` class method.  It was
..   never an API, it is only used by ``pyramid_zcml`` and its functionality has
..   been moved to that package's latest release.  This means that you'll need
..   to use the 0.9.2 or later release of ``pyramid_zcml`` with this release of
..   Pyramid.

- ``pyramid.config.Configurator.with_context`` クラスメソッドが削除され
  ました。これは API ではなく、単に ``pyramid_zcml`` によって使用されて
  いました。また、その機能は ``pyramid_zcml`` パッケージの最新版に移動
  されました。このことは、 Pyramid のこのリリースと共に ``pyramid_zcml``
  のリリース 0.9.2 以降を使用する必要があるということを意味します。


.. - The older deprecated ``set_notfound_view`` Configurator method is now an
..   alias for the new ``add_notfound_view`` Configurator method.  Likewise, the
..   older deprecated ``set_forbidden_view`` is now an alias for the new
..   ``add_forbidden_view`` Configurator method. This has the following impact:
..   the ``context`` sent to views with a ``(context, request)`` call signature
..   registered via the ``set_notfound_view`` or ``set_forbidden_view`` will now
..   be an exception object instead of the actual resource context found.  Use
..   ``request.context`` to get the actual resource context.  It's also
..   recommended to disuse ``set_notfound_view`` in favor of
..   ``add_notfound_view``, and disuse ``set_forbidden_view`` in favor of
..   ``add_forbidden_view`` despite the aliasing.

- 古い廃止された ``set_notfound_view`` Configurator メソッドは新しい
  ``add_notfound_view`` Configurator メソッドの別名になりました。同様に、
  古い廃止された ``set_forbidden_view`` は新しい ``add_forbidden_view``
  Configurator メソッドの別名になりました。これには次の影響があります:
  ``set_notfound_view`` または ``set_forbidden_view`` によって登録された
  ``(context, request)`` 呼び出し署名を持つビューに送られる
  ``context`` は、見つかった実際のリソースコンテキストではなく例外
  オブジェクトになるでしょう。実際のリソースコンテキストを得るためには
  ``request.context`` を使用してください。さらに、エイリアスされていた
  としても、 ``set_notfound_view`` を使わずに ``add_notfound_view`` を
  使うこと、 ``set_forbidden_view`` を使わずに ``add_forbidden_view`` を
  使うことが推奨されます。


.. Deprecations

廃止
------------

.. - The API documentation for ``pyramid.view.append_slash_notfound_view`` and
..   ``pyramid.view.AppendSlashNotFoundViewFactory`` was removed.  These names
..   still exist and are still importable, but they are no longer APIs.  Use
..   ``pyramid.config.Configurator.add_notfound_view(append_slash=True)`` or
..   ``pyramid.view.notfound_view_config(append_slash=True)`` to get the same
..   behavior.

- ``pyramid.view.append_slash_notfound_view`` および
  ``pyramid.view.AppendSlashNotFoundViewFactory`` の API ドキュメンテー
  ションが削除されました。これらの名前はまだ存在し、インポート可能ですが、
  それらはもはや API ではありません。同じ振る舞いを得るために
  ``pyramid.config.Configurator.add_notfound_view(append_slash=True)``
  あるいは ``pyramid.view.notfound_view_config(append_slash=True)`` を
  使用してください。


.. - The ``set_forbidden_view`` and ``set_notfound_view`` methods of the
..   Configurator were removed from the documentation.  They have been
..   deprecated since Pyramid 1.1.

- Configurator の ``set_forbidden_view`` と ``set_notfound_view`` メソッド
  がドキュメンテーションから削除されました。それらは Pyramid 1.1 以降
  廃止されていました。


.. - All references to the ``tmpl_context`` request variable were removed from
..   the docs.  Its existence in Pyramid is confusing for people who were never
..   Pylons users.  It was added as a porting convenience for Pylons users in
..   Pyramid 1.0, but it never caught on because the Pyramid rendering system is
..   a lot different than Pylons' was, and alternate ways exist to do what it
..   was designed to offer in Pylons.  It will continue to exist "forever" but
..   it will not be recommended or mentioned in the docs.

- ``tmpl_context`` リクエスト変数に対するすべての言及はドキュメントから
  削除されました。 Pyramid におけるその存在は、 Pylons ユーザでなかった
  人々を混乱させます。それは Pyramid 1.0 で Pylons ユーザの移行の利便性の
  ために追加されましたが、 Pyramid のレンダリングシステムは Pylons
  のものとは非常に異なっているため人気を得ませんでした。また、Pylons
  で実現しようとしていたことを行うための代替の方法が存在します。
  この機能は「永久に」存在し続けるでしょうが、ドキュメントの中では推奨、
  または言及されません。


.. Known Issues

既知の問題
------------

.. - As of this writing (the release of Pyramid 1.3b2), if you attempt to
..   install a Pyramid project that used the ``alchemy`` scaffold via ``setup.py
..   develop`` on Python 3.2, it will quit with an installation error while
..   trying to install ``Pygments``.  If this happens, please just rerun the
..   ``setup.py develop`` command again, and it will complete successfully.
..   This is due to a minor bug in SQLAlchemy 0.7.5 under Python 3, and will be
..   fixed in a later SQLAlchemy release.  Keep an eye on
..   http://www.sqlalchemy.org/trac/ticket/2421

- この記述 (Pyramid 1.3b2 のリリース) の時点で、 Python 3.2 上で
  ``setup.py develop`` によって ``alchemy`` scaffold を使用した
  Pyramid プロジェクトをインストールしようとした場合、 ``Pygments`` を
  インストールする間にインストールエラーで中止します。これが起こる
  場合、単に ``setup.py develop`` を再実行してください。そうすれば完全
  に成功するでしょう。これは Pythoon 3 の下の SQLAlchemy 0.7.5 の中の
  マイナーなバグによるもので、新しい SQLAlchemy リリースで修正されるで
  しょう。 http://www.sqlalchemy.org/trac/ticket/2421 を注視していてく
  ださい。


.. Documentation Enhancements

ドキュメントの改善
--------------------------

.. - The :ref:`bfg_sql_wiki_tutorial` has been updated.  It now uses
..   ``@view_config`` decorators and an explicit database population script.

- :ref:`bfg_sql_wiki_tutorial` が更新されました。それは
  ``@view_config`` デコレータと明示的なデータベースデータ投入スクリプト
  を使用するようになりました。


.. - Minor updates to the :ref:`bfg_wiki_tutorial`.

- :ref:`bfg_wiki_tutorial` のマイナーアップデート。


.. - A narrative documentation chapter named :ref:`extconfig_narr` was added; it
..   describes how to add a custom :term:`configuration directive`, and how use
..   the :meth:`pyramid.config.Configurator.action` method within custom
..   directives.  It also describes how to add :term:`introspectable` objects.

- narrative ドキュメントに :ref:`extconfig_narr` という章が追加されました;
  それは、カスタム設定ディレクティブを加える方法とカスタムディレクティブ
  内で :meth:`pyramid.config.Configurator.action` メソッドを使う方法に
  ついて記述します。さらに、 :term:`introspectable` なオブジェクトを加える
  方法について記述します。


.. - A narrative documentation chapter named :ref:`using_introspection` was
..   added.  It describes how to query the introspection system.

- narrative ドキュメントに :ref:`using_introspection` という章が追加
  されました。それは、 introspection システムに対して問い合わせる方法を
  記述します。


.. - Added an API docs chapter for :mod:`pyramid.scaffolds`.

- API ドキュメントに :mod:`pyramid.scaffolds` のための章が追加されました。


.. - Added a narrative docs chapter named :ref:`scaffolding_chapter`.

- narrative ドキュメントに :ref:`scaffolding_chapter` という章が追加されました。


.. - Added a description of the ``prequest`` command-line script at
..   :ref:`invoking_a_request`.

- :ref:`invoking_a_request` に ``prequest`` コマンドラインスクリプト
  の記述が追加されました。


.. - Added a section to the "Command-Line Pyramid" chapter named
..   :ref:`making_a_console_script`.

- "Command-Line Pyramid" 章に :ref:`making_a_console_script` という節が
  追加されました。


.. - Removed the "Running Pyramid on Google App Engine" tutorial from the main
..   docs.  It survives on in the Cookbook
..   (http://docs.pylonsproject.org/projects/pyramid_cookbook/en/latest/gae.html).
..   Rationale: it provides the correct info for the Python 2.5 version of GAE
..   only, and this version of Pyramid does not support Python 2.5.

- "Running Pyramid on Google App Engine" チュートリアルが主要ドキュメント
  から除かれました。それはクックブックの中に残されています
  (http://docs.pylonsproject.org/projects/pyramid_cookbook/en/latest/gae.html) 。
  根拠: このドキュメントは GAE の Python 2.5 バージョンでのみ正しい情報を
  提供します。そして Pyramid のこのバージョンは Python 2.5 をサポートしません。


.. - Updated the :ref:`changing_the_forbidden_view` section, replacing
..   explanations of registering a view using ``add_view`` or ``view_config``
..   with ones using ``add_forbidden_view`` or ``forbidden_view_config``.

- :ref:`changing_the_forbidden_view` 節が更新されました。
  ``add_view`` または ``view_config`` を使用してビューを登録する説明が
  ``add_forbidden_view`` または ``forbidden_view_config`` を使用する説明に
  置き換えられました。


.. - Updated the :ref:`changing_the_notfound_view` section, replacing
..   explanations of registering a view using ``add_view`` or ``view_config``
..   with ones using ``add_notfound_view`` or ``notfound_view_config``.

- :ref:`changing_the_notfound_view` 節が更新されました。
  ``add_view`` または ``view_config`` を使用してビューを登録する説明が
  ``add_notfound_view`` または ``notfound_view_config`` を使用する説明に
  置き換えられました。


.. - Updated the :ref:`redirecting_to_slash_appended_routes` section, replacing
..   explanations of registering a view using ``add_view`` or ``view_config``
..   with ones using ``add_notfound_view`` or ``notfound_view_config``

- :ref:`redirecting_to_slash_appended_routes` 節が更新されました。
  ``add_view`` あるいは ``view_config`` を使用してビューを登録する説明が
  ``add_notfound_view`` または ``notfound_view_config`` を使用する説明に
  置き換えられました。


.. - Updated all tutorials to use ``pyramid.view.forbidden_view_config`` rather
..   than ``pyramid.view.view_config`` with an HTTPForbidden context.

- すべてのチュートリアルが ``pyramid.view.view_config`` と
  HTTPForbidden コンテキストを使うのではなく
  ``pyramid.view.forbidden_view_config`` を使うように更新されました。


.. Dependency Changes

依存性の変更
------------------

.. - Pyramid no longer depends on the ``zope.component`` package, except as a
..   testing dependency.

- Pyramid は、テストのための依存性を除いて ``zope.component``
  パッケージに依存しなくなりました。


.. - Pyramid now depends on the following package versions:
..   zope.interface>=3.8.0, WebOb>=1.2dev, repoze.lru>=0.4,
..   zope.deprecation>=3.5.0, translationstring>=0.4 for Python 3 compatibility
..   purposes.  It also, as a testing dependency, depends on WebTest>=1.3.1 for
..   the same reason.

- Pyramid は次のパッケージバージョンに依存するようになりました:
  Python 3 互換性の目的のために
  zope.interface>=3.8.0, WebOb>=1.2dev, repoze.lru>=0.4,
  zope.deprecation 3.5.0, translationstring 0.4 。
  さらに、テスト依存性として、同じ理由で WebTest 1.3.1 に依存します。


.. - Pyramid no longer depends on the ``Paste`` or ``PasteScript`` packages.
..   These packages are not Python 3 compatible.

- Pyramid は ``Paste`` または ``PasteScript`` パッケージに依存しなく
  なりました。これらのパッケージは Python 3 互換ではありません。


.. - Depend on ``venusian`` >= 1.0a3 to provide scan ``ignore`` support.

- スキャン ``ignore`` サポートを提供するため ``venusian`` >= 1.0a3 に依存し
  ています。


.. Scaffolding Changes

scaffold の変更
-------------------

.. - Rendered scaffolds have now been changed to be more relocatable (fewer
..   mentions of the package name within files in the package).

- 生成された scaffold は、より再配置可能 (パッケージ中のファイル内でパッ
  ケージ名に言及する箇所が少数) になるように変更されました。


.. - The ``routesalchemy`` scaffold has been renamed ``alchemy``, replacing the
..   older (traversal-based) ``alchemy`` scaffold (which has been retired).

- ``routesalchemy`` scaffold は ``alchemy`` と改名され、より古い (トラ
  バーサルに基づいた) ``alchemy`` scaffold を置き代えました (古い
  ``alchemy`` scaffold は引退しました)。


.. - The ``alchemy`` and ``starter`` scaffolds are Python 3 compatible.

- ``alchemy`` と ``starter`` scaffold は Python 3 互換です。


.. - The ``starter`` scaffold now uses URL dispatch by default.

- ``starter`` scaffold は、デフォルトで URL ディスパッチを使用するよう
  になりました。
