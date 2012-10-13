.. What's New In Pyramid 1.4

Pyramid 1.4 の新機能
=========================

.. This article explains the new features in :app:`Pyramid` version 1.4 as
.. compared to its predecessor, :app:`Pyramid` 1.3.  It also documents backwards
.. incompatibilities between the two versions and deprecations added to
.. :app:`Pyramid` 1.4, as well as software dependency changes and notable
.. documentation additions.

この文書では :app:`Pyramid` バージョン 1.4 をその前身である
:app:`Pyramid` 1.3 を比較した場合の新機能について説明します。また、 2 つの
バージョン間の後方非互換性と :app:`Pyramid` 1.4 に加えられた deprecation
(廃止予定) に加えて、ソフトウェア依存関係の変更とドキュメンテーションの
顕著な追加分をドキュメント化します。


.. Major Feature Additions

メジャー機能追加
-----------------------

.. The major feature additions in Pyramid 1.4 follow.

Pyramid 1.4 の主な追加機能は以下の通りです。


.. Third-Party Predicates

サードパーティ述語
~~~~~~~~~~~~~~~~~~~~~~~

.. - Third-party custom view, route, and subscriber predicates can now be added
..   for use by view authors via
..   :meth:`pyramid.config.Configurator.add_view_predicate`,
..   :meth:`pyramid.config.Configurator.add_route_predicate` and
..   :meth:`pyramid.config.Configurator.add_subscriber_predicate`.  So, for
..   example, doing this:

- ビューの作成者は、サードパーティのカスタムビュー、 route 、 subscriber
  述語を :meth:`pyramid.config.Configurator.add_view_predicate`,
  :meth:`pyramid.config.Configurator.add_route_predicate`,
  :meth:`pyramid.config.Configurator.add_subscriber_predicate` を用いて
  追加して使うことができるようになりました。そのため、例えば:


  ::

     config.add_view_predicate('abc', my.package.ABCPredicate)


  .. Might allow a view author to do this in an application that configured that
  .. predicate:

  このようにすると、その述語を構成したアプリケーション内でビューの作成者が
  以下のようにすることが可能になります:


  ::

     @view_config(abc=1)


  .. Similar features exist for :meth:`pyramid.config.Configurator.add_route`,
  .. and :meth:`pyramid.config.Configurator.add_subscriber`.  See
  .. :ref:`registering_thirdparty_predicates` for more information.

  同様の機能は :meth:`pyramid.config.Configurator.add_route` および
  :meth:`pyramid.config.Configurator.add_subscriber` として存在します。
  詳細は :ref:`registering_thirdparty_predicates` を参照してください。


.. Easy Custom JSON Serialization

容易なカスタム JSON シリアライズ
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. - Views can now return custom objects which will be serialized to JSON by a
..   JSON renderer by defining a ``__json__`` method on the object's class. This
..   method should return values natively serializable by ``json.dumps`` (such
..   as ints, lists, dictionaries, strings, and so forth).  See
..   :ref:`json_serializing_custom_objects` for more information.  The JSON
..   renderer now also allows for the definition of custom type adapters to
..   convert unknown objects to JSON serializations, in case you can't add a
..   ``__json__`` method to returned objects.

- ビューがカスタムオブジェクトを返すことができるようになりました。
  カスタムオブジェクトは、そのオブジェクトのクラスに ``__json__`` メソッド
  を定義することにより JSON レンダラーによって JSON にシリアライズされます。
  このメソッドは ``json.dumps`` によってネイティブにシリアライズできる値
  (int, list, 辞書, 文字列など) を返さなければなりません。
  詳細は :ref:`json_serializing_custom_objects` を参照してください。
  返されたオブジェクトに ``__json__`` メソッドを追加することができない場合、
  さらに JSON レンダラーは未知のオブジェクトを JSON シリアライズに変換
  するためのカスタム型アダプタを定義することができるようになりました。


.. Partial Mako and Chameleon Template Renderings

Mako と Chameleon の部分的なテンプレートレンダリング
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. - The Mako renderer now supports using a def name in an asset spec.  When the
..   def name is present in the asset spec, the system will render the template
..   named def within the template instead of rendering the entire template. An
..   example asset spec which names a def is
..   ``package:path/to/template#defname.mako``. This will render the def named
..   ``defname`` inside the ``template.mako`` template instead of rendering the
..   entire template.  The old way of returning a tuple in the form
..   ``('defname', {})`` from the view is supported for backward compatibility.

- Mako レンダラーが asset spec 中での def 名の使用をサポートするように
  なりました。 asset spec 中に def 名がある場合、システムはテンプレート
  全体をレンダリングする代わりに、テンプレート内の def で指定された
  テンプレートをレンダリングします。 def を指定する asset spec の例は
  ``package:path/to/template#defname.mako`` です。これは、テンプレート
  全体をレンダリングする代わりに ``template.mako`` テンプレートの内部の
  ``defname`` という名前の def をレンダリングします。
  ビューから ``('defname', {})`` 形式でタプルを返す古い方法は、後方互換性
  のためにサポートされます。


.. - The Chameleon ZPT renderer now supports using a macro name in an asset
..   spec.  When the macro name is present in the asset spec, the system will
..   render the macro listed as a ``define-macro`` and return the result instead
..   of rendering the entire template.  An example asset spec:
..   ``package:path/to/template#macroname.pt``.  This will render the macro
..   defined as ``macroname`` within the ``template.pt`` template instead of the
..   entire templae.

- Chameleon ZPT レンダラーが asset spec 中でのマクロ名の使用をサポート
  するようになりました。 asset spec 中にマクロ名がある場合、システム
  はテンプレート全体をレンダリングする代わりに ``define-macro`` として
  リストされたマクロをレンダリングして結果を返します。
  asset spec の例: ``package:path/to/template#macroname.pt`` 。
  これは、テンプレート全体の代わりに ``template.pt`` テンプレート中で
  ``macroname`` として定義されたマクロをレンダリングします。


.. Subrequest Support

サブリクエストサポート
~~~~~~~~~~~~~~~~~~~~~~

.. - Developers may invoke a subrequest by using the
..   :meth:`pyramid.request.Request.invoke_subrequest` API.  This allows a
..   developer to obtain a response from one view callable by issuing a subrequest
..   from within a different view callable.  See :ref:`subrequest_chapter` for
..   more information.

- 開発者は :meth:`pyramid.request.Request.invoke_subrequest` API を使用
  してサブリクエストを起動することができます。これによって、開発者は
  あるビュー callable の内部からサブリクエストを呼び出すことによって
  別のビュー callable からのレスポンスを得ることができるようになります。
  詳細は :ref:`subrequest_chapter` を参照してください。


.. Minor Feature Additions

マイナー機能追加
-----------------------

.. - :meth:`pyramid.config.Configurator.add_directive` now accepts arbitrary
..   callables like partials or objects implementing ``__call__`` which don't
..   have ``__name__`` and ``__doc__`` attributes.  See
..   https://github.com/Pylons/pyramid/issues/621 and
..   https://github.com/Pylons/pyramid/pull/647.

- :meth:`pyramid.config.Configurator.add_directive` は、 partial や
  ``__call__`` を実装したオブジェクトのように ``__name__`` および
  ``__doc__`` 属性を持たない任意の callable を受け取るようになりました。
  https://github.com/Pylons/pyramid/issues/621 と
  https://github.com/Pylons/pyramid/pull/647 を参照してください。


.. - As of this release, the ``request_method`` view/route predicate, when used,
..   will also imply that ``HEAD`` is implied when you use ``GET``.  For
..   example, using ``@view_config(request_method='GET')`` is equivalent to
..   using ``@view_config(request_method=('GET', 'HEAD'))``.  Using
..   ``@view_config(request_method=('GET', 'POST')`` is equivalent to using
..   ``@view_config(request_method=('GET', 'HEAD', 'POST')``.  This is because
..   HEAD is a variant of GET that omits the body, and WebOb has special support
..   to return an empty body when a HEAD is used.

- このリリースから、 ``request_method`` ビュー/route 述語が使われている場合に
  ``GET`` が指定されたら ``HEAD`` も指定されたものとみなすようになりました。
  例えば、 ``@view_config(request_method='GET')`` を使うことは
  ``@view_config(request_method=('GET', 'HEAD'))`` を使うことと等価です。
  ``@view_config(request_method=('GET', 'POST')`` を使うことは
  ``@view_config(request_method=('GET', 'HEAD', 'POST')`` を使うことと等価です。
  これは HEAD が body を省略した GET の変種であるためです。また、 WebOb には
  HEAD が使用された場合に空の body を返すための特別なサポートがあります。


.. - :meth:`pyramid.config.Configurator.add_request_method` has been introduced
..   to support extending request objects with arbitrary callables. This method
..   expands on the now documentation-deprecated
..   :meth:`pyramid.config.Configurator.set_request_property` by supporting
..   methods as well as properties. This method also causes less code to be
..   executed at request construction time than
..   :meth:`~pyramid.config.Configurator.set_request_property`.

- 任意の callable によるリクエストオブジェクトの拡張をサポートするために
  :meth:`pyramid.config.Configurator.add_request_method` が導入されました。
  このメソッドは、プロパティだけでなくメソッドもサポートすることで、
  今では deprecated とドキュメント化された
  :meth:`pyramid.config.Configurator.set_request_property` を拡張します。
  さらにこのメソッドは、リクエスト生成時に
  :meth:`~pyramid.config.Configurator.set_request_property` と比べて
  より少ないコードを実行します。


.. - The static view machinery now raises rather than returns
..   :class:`pyramid.httpexceptions.HTTPNotFound` and
..   :class:`pyramid.httpexceptions.HTTPMovedPermanently` exceptions, so these can
..   be caught by the notfound view (and other exception views).

- 静的ビュー機構は :class:`pyramid.httpexceptions.HTTPNotFound` および
  :class:`pyramid.httpexceptions.HTTPMovedPermanently` 例外を戻り値として
  返すのではなく例外として投げるようになりました。そのため、これらの例外を
  notfound ビュー (または他の例外ビュー) によって捕捉することができます。


.. - When there is a predicate mismatch exception (seen when no view matches for
..   a given request due to predicates not working), the exception now contains
..   a textual description of the predicate which didn't match.

- 述語不一致例外が起きた場合 (述語が働かないために与えられたリクエストに
  一致するビューがない場合に見られる) 、一致しなかった述語に関するテキストの
  説明が例外に含まれるようになりました。


.. - An :meth:`pyramid.config.Configurator.add_permission` directive method was
..   added to the Configurator.  This directive registers a free-standing
..   permission introspectable into the Pyramid introspection system.
..   Frameworks built atop Pyramid can thus use the the ``permissions``
..   introspectable category data to build a comprehensive list of permissions
..   supported by a running system.  Before this method was added, permissions
..   were already registered in this introspectable category as a side effect of
..   naming them in an :meth:`pyramid.config.Configurator.add_view` call, this
..   method just makes it possible to arrange for a permission to be put into
..   the ``permissions`` introspectable category without naming it along with an
..   associated view.  Here's an example of usage of ``add_permission``:

- :meth:`pyramid.config.Configurator.add_permission` ディレクティブ
  メソッドが Configurator に追加されました。このディレクティブは、
  独立した内省可能なパーミッションを Pyramid 内省システムに登録します。
  Pyramid 上に構築されたフレームワークは、したがって実行中のシステムで
  サポートされているパーミッションの完全なリストを構築するために
  ``permissions`` introspectable カテゴリーデータを使用できます。
  このメソッドが追加される以前にも、
  :meth:`pyramid.config.Configurator.add_view` 呼び出しで
  パーミッションに名前を付けることの副作用として
  この introspectable カテゴリーにパーミッションが登録されていました。
  このメソッドは、単に関連するビューと共にパーミッションに名前を付ける
  ことなく ``permissions`` introspectable カテゴリーにパーミッションを
  入れられるようにします。これは ``add_permission`` の使用の一例です:


  ::

      config = Configurator()
      config.add_permission('view')


.. - The :func:`pyramid.session.UnencryptedCookieSessionFactoryConfig` function
..   now accepts ``signed_serialize`` and ``signed_deserialize`` hooks which may
..   be used to influence how the sessions are marshalled (by default this is
..   done with HMAC+pickle).

- :func:`pyramid.session.UnencryptedCookieSessionFactoryConfig` 関数が、
  セッションの直列化の方法 (デフォルトでは、 HMAC+pickle によって行われます)
  に影響を与えるために使用できる ``signed_serialize`` および
  ``signed_deserialize`` フックを受け取るようになりました。


.. - :class:`pyramid.testing.DummyRequest` now supports methods supplied by the
..   ``pyramid.util.InstancePropertyMixin`` class such as ``set_property``.

- :class:`pyramid.testing.DummyRequest` が、
  ``pyramid.util.InstancePropertyMixin`` クラスによって提供される
  ``set_property`` のようなメソッドをサポートするようになりました。


.. - Request properties and methods added via
..   :meth:`pyramid.config.Configurator.add_request_method` or
..   :meth:`pyramid.config.Configurator.set_request_property` are now available to
..   tweens.

- :meth:`pyramid.config.Configurator.add_request_method` や
  :meth:`pyramid.config.Configurator.set_request_property` によって追加
  されたリクエストプロパティおよびメソッドが、 tween でも利用可能になり
  ました。


.. - Request properties and methods added via
..   :meth:`pyramid.config.Configurator.add_request_method` or
..   :meth:`pyramid.config.Configurator.set_request_property` are now available
..   in the request object returned from :func:`pyramid.paster.bootstrap`.

- :meth:`pyramid.config.Configurator.add_request_method` や
  :meth:`pyramid.config.Configurator.set_request_property` によって追加
  されたリクエストプロパティおよびメソッドが、
  :func:`pyramid.paster.bootstrap` から返されたリクエストオブジェクトでも
  利用可能になりました。


.. - ``request.context`` of environment request during
..   :func:`pyramid.paster.bootstrap` is now the root object if a context isn't
..   already set on a provided request.

- :func:`pyramid.paster.bootstrap` 中に、提供されたリクエストにまだ
  コンテキストがセットされていない場合、環境リクエストの
  ``request.context`` が root オブジェクトとなるようになりました。


.. - :class:`pyramid.decorator.reify`  is now an API, and was added to
..   the API documentation.

- :class:`pyramid.decorator.reify` が API になり、 API ドキュメンテーション
  に追加されました。


.. - Added the :func:`pyramid.testing.testConfig` context manager, which can be
..   used to generate a configurator in a test, e.g. ``with
..   testing.testConfig(...):``.

- :func:`pyramid.testing.testConfig` コンテキストマネージャーが追加されました。
  これは、例えば ``with testing.testConfig(...):`` のようにして、
  テストでコンフィギュレータを生成するために使用することができます。


.. - A new :func:`pyramid.session.check_csrf_token` convenience API function was
..   added.

- 利便性のための :func:`pyramid.session.check_csrf_token` 新 API 関数が
  追加されました。


.. - A ``check_csrf`` view predicate was added.  For example, you can now do
..   ``config.add_view(someview, check_csrf=True)``.  When the predicate is
..   checked, if the ``csrf_token`` value in ``request.params`` matches the csrf
..   token in the request's session, the view will be permitted to execute.
..   Otherwise, it will not be permitted to execute.

- ``check_csrf`` ビュー述語が追加されました。これにより、例えば
  ``config.add_view(someview, check_csrf=True)`` ができるようになります。
  この述語がチェックされた時に、 ``request.params`` 中の ``csrf_token`` の
  値がリクエストのセッション中の csrf トークンと一致すればビューは実行を
  許可されます。そうでなければ実行は許可されません。


.. - Add ``Base.metadata.bind = engine`` to ``alchemy`` scaffold, so that tables
..   defined imperatively will work.

- 命令的に定義されたテーブルが動作するように、
  ``Base.metadata.bind = engine`` が ``alchemy`` scaffold に追加されました。


.. Backwards Incompatibilities

後方非互換性
---------------------------

.. - The Pyramid router no longer adds the values ``bfg.routes.route`` or
..   ``bfg.routes.matchdict`` to the request's WSGI environment dictionary.
..   These values were docs-deprecated in ``repoze.bfg`` 1.0 (effectively seven
..   minor releases ago).  If your code depended on these values, use
..   request.matched_route and request.matchdict instead.

- Pyramid ルーターは、リクエストの WSGI 環境辞書に値
  ``bfg.routes.route`` や ``bfg.routes.matchdict`` を追加しなくなりました。
  これらの値は ``repoze.bfg`` 1.0 (事実上マイナーリリース 7 つ前) で
  deprecated とドキュメント化されています。
  あなたのコードがこれらの値に依存している場合は、代わりに
  request.matched_route と request.matchdict を使用してください。


.. - It is no longer possible to pass an environ dictionary directly to
..   ``pyramid.traversal.ResourceTreeTraverser.__call__`` (aka
..   ``ModelGraphTraverser.__call__``).  Instead, you must pass a request
..   object.  Passing an environment instead of a request has generated a
..   deprecation warning since Pyramid 1.1.

- ``pyramid.traversal.ResourceTreeTraverser.__call__``
  (別名 ``ModelGraphTraverser.__call__``) に environ 辞書を直接渡すことは
  できなくなりました。代わりにリクエストオブジェクトを渡す必要があります。
  リクエストの代わりに environ を渡すと、 Pyramid 1.1 からは deprecation
  警告が発生していました。


.. - Pyramid will no longer work properly if you use the
..   ``webob.request.LegacyRequest`` as a request factory.  Instances of the
..   LegacyRequest class have a ``request.path_info`` which return a string.
..   This Pyramid release assumes that ``request.path_info`` will
..   unconditionally be Unicode.

- リクエストファクトリとして ``webob.request.LegacyRequest`` を使用している
  場合、 Pyramid は正常に動作しなくなりました。 LegacyRequest クラスの
  インスタンスには文字列を返す ``request.path_info`` があり、Pyramid の
  このリリースは ``request.path_info`` が無条件でユニコードであることを
  仮定しています。


.. - The functions from ``pyramid.chameleon_zpt`` and ``pyramid.chameleon_text``
..   named ``get_renderer``, ``get_template``, ``render_template``, and
..   ``render_template_to_response`` have been removed.  These have issued a
..   deprecation warning upon import since Pyramid 1.0.  Use
..   :func:`pyramid.renderers.get_renderer`,
..   ``pyramid.renderers.get_renderer().implementation()``,
..   :func:`pyramid.renderers.render` or
..   :func:`pyramid.renderers.render_to_response` respectively instead of these
..   functions.

- ``pyramid.chameleon_zpt`` と ``pyramid.chameleon_text`` から
  ``get_renderer``, ``get_template``, ``render_template``,
  ``render_template_to_response`` という名前の関数が削除されました。
  これらは Pyramid 1.0 からインポート時に deprecation 警告が出ていました。
  これらの関数の代わりに、 :func:`pyramid.renderers.get_renderer`,
  ``pyramid.renderers.get_renderer().implementation()``,
  :func:`pyramid.renderers.render`,
  :func:`pyramid.renderers.render_to_response` をそれぞれ使用してください。


.. - The ``pyramid.configuration`` module was removed.  It had been deprecated
..   since Pyramid 1.0 and printed a deprecation warning upon its use.  Use
..   :mod:`pyramid.config` instead.

- ``pyramid.configuration`` モジュールが削除されました。このモジュールは
  Pyramid 1.0 から deprecated になり、使用すると deprecation 警告が表示
  されていました。代わりに :mod:`pyramid.config` を使用してください。


.. - The ``pyramid.paster.PyramidTemplate`` API was removed.  It had been
..   deprecated since Pyramid 1.1 and issued a warning on import.  If your code
..   depended on this, adjust your code to import
..   :class:`pyramid.scaffolds.PyramidTemplate` instead.

- ``pyramid.paster.PyramidTemplate`` API が削除されました。それは
  Pyramid 1.1 から deprecated になり、インポート時に警告が出ていました。
  あなたのコードがこれに依存している場合、代わりに
  :class:`pyramid.scaffolds.PyramidTemplate` をインポートするように
  コードを修正してください。


.. - The ``pyramid.settings.get_settings()`` API was removed.  It had been
..   printing a deprecation warning since Pyramid 1.0.  If your code depended on
..   this API, use ``pyramid.threadlocal.get_current_registry().settings``
..   instead or use the ``settings`` attribute of the registry available from
..   the request (``request.registry.settings``).

- ``pyramid.settings.get_settings()`` API が削除されました。この API は
  Pyramid 1.0 から deprecation 警告が表示されていました。あなたのコードが
  この API に依存している場合は、代わりに
  ``pyramid.threadlocal.get_current_registry().settings`` を使用するか、
  リクエストからアクセス可能なレジストリの ``settings`` 属性
  (``request.registry.settings``) を使用してください。


.. - These APIs from the ``pyramid.testing`` module were removed.  They have
..   been printing deprecation warnings since Pyramid 1.0:

- ``pyramid.testing`` モジュールから以下の API が削除されました。
  これらは Pyramid 1.0 から deprecation 警告が表示されていました:


  .. * ``registerDummySecurityPolicy``, use
  ..   :meth:`pyramid.config.Configurator.testing_securitypolicy` instead.

  * ``registerDummySecurityPolicy``, 代わりに
    :meth:`pyramid.config.Configurator.testing_securitypolicy`
    を使ってください。


  .. * ``registerResources`` (aka ``registerModels``), use
  ..   :meth:`pyramid.config.Configurator.testing_resources` instead.

  * ``registerResources`` (別名 ``registerModels``), 代わりに
    :meth:`pyramid.config.Configurator.testing_resources`
    を使ってください。


  .. * ``registerEventListener``, use
  ..   :meth:`pyramid.config.Configurator.testing_add_subscriber` instead.

  * ``registerEventListener``, 代わりに
    :meth:`pyramid.config.Configurator.testing_add_subscriber`
    を使ってください。


  .. * ``registerTemplateRenderer`` (aka `registerDummyRenderer``), use
  ..   :meth:`pyramid.config.Configurator.testing_add_template` instead.

  * ``registerTemplateRenderer`` (別名 ``registerDummyRenderer``), 代わりに
    :meth:`pyramid.config.Configurator.testing_add_template`
    を使ってください。


  .. * ``registerView``, use :meth:`pyramid.config.Configurator.add_view` instead.

  * ``registerView``, 代わりに :meth:`pyramid.config.Configurator.add_view`
    を使ってください。


  .. * ``registerUtility``, use
  ..   :meth:`pyramid.config.Configurator.registry.registerUtility` instead.

  * ``registerUtility``, 代わりに
    :meth:`pyramid.config.Configurator.registry.registerUtility`
    を使ってください。


  .. * ``registerAdapter``, use
  ..   :meth:`pyramid.config.Configurator.registry.registerAdapter` instead.

  * ``registerAdapter``, 代わりに
    :meth:`pyramid.config.Configurator.registry.registerAdapter`
    を使ってください。


  .. * ``registerSubscriber``, use 
  ..   :meth:`pyramid.config.Configurator.add_subscriber` instead.

  * ``registerSubscriber``, 代わりに
    :meth:`pyramid.config.Configurator.add_subscriber`
    を使ってください。


  .. * ``registerRoute``, use 
  ..   :meth:`pyramid.config.Configurator.add_route` instead.

  * ``registerRoute``, 代わりに
    :meth:`pyramid.config.Configurator.add_route`
    を使ってください。


  .. * ``registerSettings``, use 
  ..   :meth:`pyramid.config.Configurator.add_settings` instead.

  * ``registerSettings``, 代わりに
    :meth:`pyramid.config.Configurator.add_settings`
    を使ってください。


.. - In Pyramid 1.3 and previous, the ``__call__`` method of a Response object
..   returned by a view was invoked before any finished callbacks were executed.
..   As of this release, the ``__call__`` method of a Response object is invoked
..   *after* finished callbacks are executed.  This is in support of the
..   :meth:`pyramid.request.Request.invoke_subrequest` feature.

- Pyramid 1.3 とそれ以前には、ビューによって返された Response オブジェクトの
  ``__call__`` メソッドがすべての finished コールバックが実行される前に
  起動されていました。このリリース以降、Response オブジェクトの
  ``__call__`` メソッドはfinished コールバックが実行された後に起動されます。
  これは :meth:`pyramid.request.Request.invoke_subrequest` 機能をサポート
  するためです。


Deprecations
------------

.. - The :meth:`pyramid.config.Configurator.set_request_property` directive has
..   been documentation-deprecated.  The method remains usable but the more
..   featureful :meth:`pyramid.config.Configurator.add_request_method` should be
..   used in its place (it has all of the same capabilities but can also extend
..   the request object with methods).

- :meth:`pyramid.config.Configurator.set_request_property` ディレクティブ
  が deprecated とドキュメント化されました。このメソッドは今でも使えますが、
  より機能豊富な :meth:`pyramid.config.Configurator.add_request_method` を
  代わりに使うことが推奨されます (それは同じ能力をすべて持っていて、さらに
  リクエストオブジェクトをメソッドによって拡張することができます)。


.. Documentation Enhancements

ドキュメントの強化
--------------------------

.. - Added an :ref:`upgrading_chapter` chapter to the narrative documentation.
..   It describes how to cope with deprecations and removals of Pyramid APIs and
..   how to show Pyramid-generated deprecation warnings while running tests and
..   while running a server.

- :ref:`upgrading_chapter` の章が narrative ドキュメンテーションに追加
  されました。この章は、 Pyramid API の deprecation と削除に対処する方法、
  テストを実行している間やサーバーを起動している間に Pyramid によって
  生成された deprecation 警告を表示する方法について記述します。


.. - Added a :ref:`subrequest_chapter` chapter to the narrative documentation.

- :ref:`subrequest_chapter` の章が narrative ドキュメンテーションに追加
  されました。


.. - Many cleanups and improvements to narrative and API docs.

- narrative および API ドキュメントに対して多くの整理と改善が行われました。


.. Dependency Changes

依存関係の変更
------------------

.. - Pyramid now requires WebOb 1.2b3+ (the prior Pyramid release only relied on
..   1.2dev+).  This is to ensure that we obtain a version of WebOb that returns
..   ``request.path_info`` as text.

- Pyramid は WebOb 1.2b3+ を要求するようになりました(以前の Pyramid
  リリースは、 1.2dev+ に依存していました) 。これは、
  ``request.path_info`` をテキストとして返す WebOb のバージョンを確実に
  得るためです。

