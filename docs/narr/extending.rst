.. Extending An Existing :app:`Pyramid` Application

.. _extending_chapter:

既存の :app:`Pyramid` アプリケーションの拡張
===================================================

.. If a :app:`Pyramid` developer has obeyed certain constraints while building
.. an application, a third party should be able to change the application's
.. behavior without needing to modify its source code.  The behavior of a
.. :app:`Pyramid` application that obeys certain constraints can be *overridden*
.. or *extended* without modification.

:app:`Pyramid` 開発者がアプリケーションを構築する際にある制約に従っていれば、
サードパーティーはそのソースコードを修正する必要なしにアプリケーションの
振る舞いを変更することができます。ある制約に従う :app:`Pyramid`
アプリケーションの振る舞いは、修正なしで *オーバーライド* または *拡張*
ができます。


.. We'll define some jargon here for the benefit of identifying the parties
.. involved in such an effort.

そのような取り組みに関係する人々を識別するために、ここで専門用語を定義します。


.. Developer
..   The original application developer.

開発者
  オリジナルのアプリケーション開発者。


.. Integrator
..   Another developer who wishes to reuse the application written by the
..   original application developer in an unanticipated context.  He may also
..   wish to modify the original application without changing the original
..   application's source code.

インテグレーター
  不測のコンテキストの中でオリジナルのアプリケーション開発者によって
  書かれたアプリケーションを再利用したい別の開発者。
  さらに、彼はオリジナルのアプリケーションのソースコードを変更せずに、
  オリジナルのアプリケーションを修正したいと思うかもしれません。


.. The Difference Between "Extensible" and "Pluggable" Applications

「拡張可能」なアプリケーションと「プラグ可能」なアプリケーションの違い
----------------------------------------------------------------------

.. Other web frameworks, such as :term:`Django`, advertise that they allow
.. developers to create "pluggable applications".  They claim that if you create
.. an application in a certain way, it will be integratable in a sensible,
.. structured way into another arbitrarily-written application or project
.. created by a third-party developer.

:term:`Django` のような他のウェブフレームワークは、開発者が
「プラグ可能なアプリケーション」を作成することができると宣伝しています。
特定の方法でアプリケーションを作成すれば、サードパーティーの開発者によって
作成された別の任意のアプリケーションあるいはプロジェクトに合理的かつ構造化
された方法でアプリケーションを統合することができると主張します。


.. :app:`Pyramid`, as a platform, does not claim to provide such a feature.  The
.. platform provides no guarantee that you can create an application and package
.. it up such that an arbitrary integrator can use it as a subcomponent in a
.. larger Pyramid application or project.  Pyramid does not mandate the
.. constraints necessary for such a pattern to work satisfactorily.  Because
.. Pyramid is not very "opinionated", developers are able to use wildly
.. different patterns and technologies to build an application.  A given Pyramid
.. application may happen to be reusable by a particular third party integrator,
.. because the integrator and the original developer may share similar base
.. technology choices (such as the use of a particular relational database or
.. ORM).  But the same application may not be reusable by a different developer,
.. because he has made different technology choices which are incompatible with
.. the original developer's.

プラットフォームとしての :app:`Pyramid` は、そのような特徴を提供すると
主張しません。プラットフォームは、任意のインテグレーターがより大きな
Pyramid アプリケーションあるいはプロジェクトの中でサブコンポーネントと
して使用できるような形でアプリケーションを作成してそれをパッケージ化
することができる、という保証をしていません。 Pyramid は、そのような
パターンが満足に働くために必要な制約を強制しません。 Pyramid はあまり
「主張が強い」フレームワークではないので、開発者はアプリケーションを
構築するために非常に異なるパターンや技術を使用することができます。
与えられた Pyramid アプリケーションは、インテグレーターとオリジナルの
開発者が (特定のリレーショナルデータベースや ORM の使用といった) 類似した
基礎技術の選択を共有しているといった理由で、運が良ければ特定のサード
パーティーインテグレーターにとっては再利用可能かもしれません。
しかし、同じアプリケーションがオリジナルの開発者と互換性のない異なる
技術選択を行なった別の開発者にとっては再利用可能ではないかもしれません。


.. As a result, the concept of a "pluggable application" is left to layers built
.. above Pyramid, such as a "CMS" layer or "application server" layer.  Such
.. layers are apt to provide the necessary "opinions" (such as mandating a
.. storage layer, a templating system, and a structured, well-documented pattern
.. of registering that certain URLs map to certain bits of code) which makes the
.. concept of a "pluggable application" possible.  "Pluggable applications",
.. thus, should not plug in to Pyramid itself but should instead plug into a
.. system written atop Pyramid.

その結果、「プラグ可能なアプリケーション」の概念は「CMS」レイヤーや
「アプリケーションサーバ」レイヤーのような Pyramid 上に構築されるレイヤー
に残されます。そのようなレイヤーは「プラグ可能なアプリケーション」の
概念を可能にする、必要な「主張」 (ストレージレイヤーや、テンプレート
システム、 URL をコードにマッピングするための構造化され十分ドキュメント化
された登録のパターンを強制することなど) を提供する傾向にあります。
このように、「プラグ可能なアプリケーション」は、 Pyramid 自体にプラグイン
されるのではなく、代わりに Pyramid の上に書かれたシステムにプラグイン
されます。


.. Although it does not provide for "pluggable applications", Pyramid *does*
.. provide a rich set of mechanisms which allows for the extension of a single
.. existing application.  Such features can be used by frameworks built using
.. Pyramid as a base.  All Pyramid applications may not be *pluggable*, but all
.. Pyramid applications are *extensible*.

Pyramid は「プラグ可能なアプリケーション」を提供しませんが、
単一の既存のアプリケーションの拡張を可能にする豊富なメカニズムのセットを
提供 *します* 。そのような特徴は、基礎として Pyramid を使用して構築された
フレームワークによって使用することができます。すべての Pyramid
アプリケーションはプラグ可能とは限りませんが、すべて Pyramid
アプリケーションは拡張可能です。


.. index::
   single: extensible application

.. Rules for Building An Extensible Application

.. _building_an_extensible_app:

拡張可能なアプリケーションを構築するためのルール
------------------------------------------------

.. There is only one rule you need to obey if you want to build a maximally
.. extensible :app:`Pyramid` application: as a developer, you should factor any
.. overrideable :term:`imperative configuration` you've created into functions
.. which can be used via :meth:`pyramid.config.Configurator.include` rather than
.. inlined as calls to methods of a :term:`Configurator` within the ``main``
.. function in your application's ``__init__.py``.  For example, rather than:

最大限に拡張可能な :app:`Pyramid` アプリケーションを構築したければ、
従うべきルールがたった1つだけあります: 開発者として、作成したあらゆる
オーバーライド可能な :term:`imperative configuration` を、アプリケーション
の ``__init__.py`` 中の ``main`` 関数内で :term:`Configurator` メソッドへの
呼び出しとしてインライン化するのではなく、
:meth:`pyramid.config.Configurator.include` によって使用できる関数に
分解すべきです。例えば、こうではなく:


.. code-block:: python
   :linenos:

   from pyramid.config import Configurator

   if __name__ == '__main__':
       config = Configurator()
       config.add_view('myapp.views.view1', name='view1')
       config.add_view('myapp.views.view2', name='view2')


.. You should do move the calls to ``add_view`` outside of the (non-reusable)
.. ``if __name__ == '__main__'`` block, and into a reusable function:

``add_view`` の呼び出しを (再利用可能でない) ``if __name__ == '__main__'``
ブロックの外部に出して、再利用可能な関数の中に移動させてください:


.. code-block:: python
   :linenos:

   from pyramid.config import Configurator

   if __name__ == '__main__':
       config = Configurator()
       config.include(add_views)

   def add_views(config):
       config.add_view('myapp.views.view1', name='view1')
       config.add_view('myapp.views.view2', name='view2')


.. Doing this allows an integrator to maximally reuse the configuration
.. statements that relate to your application by allowing him to selectively
.. include or disinclude the configuration functions you've created from an
.. "override package".

これによって、インテグレーターがあなたの作成した設定関数を「オーバーライド
パッケージ」から取捨選択 (selectively include or disinclude) することで
あなたのアプリケーションに関係する設定命令を最大限に再利用できるようになります。


.. Alternately, you can use :term:`ZCML` for the purpose of making configuration
.. extensible and overrideable. :term:`ZCML` declarations that belong to an
.. application can be overridden and extended by integrators as necessary in a
.. similar fashion.  If you use only :term:`ZCML` to configure your application,
.. it will automatically be maximally extensible without any manual effort.  See
.. :term:`pyramid_zcml` for information about using ZCML.

別の方法として、設定を拡張可能かつオーバーライド可能にする目的で :term:`ZCML`
を使用することもできます。アプリケーションに属する :term:`ZCML` 宣言は、
必要なときに同様の方法でインテグレーターによってオーバーライドしたり拡張したり
することができます。アプリケーションを設定するために :term:`ZCML` を
使用するだけで、手作業による労力なしで自動的に最大限に拡張可能になります。
ZCML の使用に関する情報については :term:`pyramid_zcml` を参照してください。


.. Fundamental Plugpoints

基本的なプラグポイント
~~~~~~~~~~~~~~~~~~~~~~

.. The fundamental "plug points" of an application developed using
.. :app:`Pyramid` are *routes*, *views*, and *assets*.  Routes are declarations
.. made using the :meth:`pyramid.config.Configurator.add_route` method.  Views
.. are declarations made using the :meth:`pyramid.config.Configurator.add_view`
.. method.  Assets are files that are
.. accessed by :app:`Pyramid` using the :term:`pkg_resources` API such as static
.. files and templates via a :term:`asset specification`.  Other directives and
.. configurator methods also deal in routes, views, and assets.  For example, the
.. ``add_handler`` directive of the ``pyramid_handlers`` package adds a single
.. route, and some number of views.

:app:`Pyramid` を使用して開発されたアプリケーションの基本的な
「プラグポイント」は、 *route*, *ビュー*, *asset* です。 route は
:meth:`pyramid.config.Configurator.add_route` メソッドを使用して行われた
宣言です。ビューは :meth:`pyramid.config.Configurator.add_view` メソッドを
使用して行われた宣言です。 asset は :term:`pkg_resources` APIを使用して
:app:`Pyramid` によってアクセスされるファイルで、例えば
:term:`asset specification` 経由でアクセスされる静的ファイルとテンプレート
などがあります。他のディレクティブや configurator メソッドもまた route,
ビュー, asset を扱います。例えば、 ``pyramid_handlers`` パッケージの
``add_handler`` ディレクティブは一つの route といくつかのビューを追加します。


.. index::
   single: extending an existing application

.. Extending an Existing Application

既存のアプリケーションの拡張
---------------------------------

.. The steps for extending an existing application depend largely on whether the
.. application does or does not use configuration decorators and/or imperative
.. code.

既存のアプリケーションを拡張するためのステップは、アプリケーションが
設定デコレータと命令型コードのどちらを (または両方を) 使用しているか
どうかに大きく依存します。


.. If The Application Has Configuration Decorations

アプリケーションが設定デコレータを使用している場合
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. You've inherited a :app:`Pyramid` application which you'd like to extend or
.. override that uses :class:`pyramid.view.view_config` decorators or other
.. :term:`configuration decoration` decorators.

あなたは拡張またはオーバーライドしたい :app:`Pyramid` アプリケーションを継承
しました。そのアプリケーションは :class:`pyramid.view.view_config`
デコレータあるいは他の :term:`configuration decoration` を使用しています。


.. If you just want to *extend* the application, you can run a :term:`scan`
.. against the application's package, then add additional configuration that
.. registers more views or routes.

アプリケーションを単に *拡張* したければ、アプリケーションパッケージに対して
:term:`scan` を実行して、次に、追加のビューまたは route を登録する設定を
追加することができます。


.. code-block:: python
   :linenos:

   if __name__ == '__main__':
       config.scan('someotherpackage')
       config.add_view('mypackage.views.myview', name='myview')


.. If you want to *override* configuration in the application, you *may* need to
.. run :meth:`pyramid.config.Configurator.commit` after performing the scan of
.. the original package, then add additional configuration that registers more
.. views or routes which performs overrides.

アプリケーションでの設定を *オーバーライド* したければ、オリジナルパッケージの
scan を行なった後で :meth:`pyramid.config.Configurator.commit` を実行
する必要がある *かもしれません* 。その後、追加のビューあるいは route を
登録する設定を追加すると、それはオーバーライドを行ないます。


.. code-block:: python
   :linenos:

   if __name__ == '__main__':
       config.scan('someotherpackage')
       config.commit()
       config.add_view('mypackage.views.myview', name='myview')


.. Once this is done, you should be able to extend or override the application
.. like any other (see :ref:`extending_the_application`).

一旦これが行われると、他と同じようにアプリケーションを拡張またはオーバーライド
することができるはずです (:ref:`extending_the_application` を参照)。


.. You can alternately just prevent a :term:`scan` from happening (by omitting
.. any call to the :meth:`pyramid.config.Configurator.scan` method).  This will
.. cause the decorators attached to objects in the target application to do
.. nothing.  At this point, you will need to convert all the configuration done
.. in decorators into equivalent imperative configuration or ZCML and add that
.. configuration or ZCML to a separate Python package as described in
.. :ref:`extending_the_application`.

別の方法で、単に :term:`scan` が起こらないようにすることもできます
(:meth:`pyramid.config.Configurator.scan` メソッドの呼び出しを省略する
ことによって)。これは対象のアプリケーション中でオブジェクトに取り付けら
れているデコレータに何もさせない結果になります。このポイントでは、
デコレータの中で行われたすべての設定を等価な命令型の設定あるいは ZCML に
変換して、その設定または ZCML を :ref:`extending_the_application` で
説明されているように個別の Python パッケージに追加する必要があるでしょう。


.. Extending the Application

.. _extending_the_application:

アプリケーションの拡張
~~~~~~~~~~~~~~~~~~~~~~~~~

.. To extend or override the behavior of an existing application, you will need
.. to create a new package which includes the configuration of the old package,
.. and you'll perhaps need to create implementations of the types of things
.. you'd like to override (such as views), which are referred to within the
.. original package.

既存のアプリケーションの振る舞いを拡張またはオーバーライドするために、
古いパッケージの設定を含む新しいパッケージを作成する必要があるでしょう。
また、恐らく (ビューのような) オリジナルのパッケージ内で参照されている
オーバーライドしたい種々のものの実装を作成する必要があるでしょう。


.. The general pattern for extending an existing application looks something
.. like this:

既存のアプリケーションの拡張に対する一般的なパターンはこのようなものです:


.. - Create a new Python package.  The easiest way to do this is to create a new
..   :app:`Pyramid` application using the scaffold mechanism.  See
..   :ref:`creating_a_project` for more information.

- 新しい Python パッケージを作成する。これを行う最も簡単な方法は scaffold
  メカニズムを使用して、新しい :app:`Pyramid` アプリケーションを作成する
  ことです。詳細は :ref:`creating_a_project` を参照してください。


.. - In the new package, create Python files containing views and other
..   overridden elements, such as templates and static assets as necessary.

- 新しいパッケージで、ビューや他のオーバーライドされた要素 (必要に応じて
  テンプレートや静的 asset のような) を含む Python ファイルを作成する。


.. - Install the new package into the same Python environment as the original
..   application (e.g. ``$myvenv/bin/python setup.py develop`` or
..   ``$myvenv/bin/python setup.py install``).

- 新しいパッケージをオリジナルアプリケーションと同じ Python 環境に
  インストールする (例 ``$myvenv/bin/python setup.py develop`` または
  ``$myvenv/bin/python setup.py install``)。


.. - Change the ``main`` function in the new package's ``__init__.py`` to include
..   the original :app:`Pyramid` application's configuration functions via
..   :meth:`pyramid.config.Configurator.include` statements or a :term:`scan`.

- 新しいパッケージの ``__init__.py`` 中の ``main`` 関数を変更して、
  :meth:`pyramid.config.Configurator.include` 文あるいは :term:`scan` に
  よってオリジナルの :app:`Pyramid` アプリケーションの設定関数をインクルードする。


.. - Wire the new views and assets created in the new package up using
..   imperative registrations within the ``main`` function of the
..   ``__init__.py`` file of the new application.  These wiring should happen
..   *after* including the configuration functions of the old application.
..   These registrations will extend or override any registrations performed by
..   the original application.  See :ref:`overriding_views`,
..   :ref:`overriding_routes` and :ref:`overriding_resources`.

- 新しいアプリケーションの ``__init__.py`` ファイルの ``main`` 関数内で
  命令型の登録を使用して、新しいパッケージ内で作成された新しいビューおよび
  asset を接続する。この接続は、古いアプリケーションの設定関数をインクルードした
  *後で* 行う必要があります。これらの登録は、オリジナルアプリケーションに
  よって行なわれた任意の登録を拡張またはオーバーライドすることができます。
  :ref:`overriding_views`, :ref:`overriding_routes`, および
  :ref:`overriding_resources` を参照してください。


.. index::
   pair: overriding; views

.. Overriding Views

.. _overriding_views:

ビューのオーバーライド
~~~~~~~~~~~~~~~~~~~~~~

.. The :term:`view configuration` declarations you make which *override*
.. application behavior will usually have the same :term:`view predicate`
.. attributes as the original you wish to override.  These ``<view>``
.. declarations will point at "new" view code, in the override package you've
.. created.  The new view code itself will usually be cut-n-paste copies of view
.. callables from the original application with slight tweaks.

あなたが行ったアプリケーションの振る舞いを *オーバーライド* する
:term:`view configuration` 宣言は、通常オーバーライドしたいオリジナル
アプリケーションと同じ :term:`view predicate` を持つでしょう。これらの
``<view>`` 宣言は、作成したオーバーライドパッケージの中にある「新しい」
ビューコードを指すでしょう。新しいビューコード自身は、通常オリジナル
アプリケーションのビュー callable のカット&ペーストによるコピーに多少の
修正を加えたものになるでしょう。


.. For example, if the original application has the following
.. ``configure_views`` configuration method:

例えば、オリジナルのアプリケーションに次のような ``configure_views``
設定メソッドがある場合:


.. code-block:: python
   :linenos:

    def configure_views(config):
        config.add_view('theoriginalapp.views.theview', name='theview')


.. You can override the first view configuration statement made by
.. ``configure_views`` within the override package, after loading the original
.. configuration function:

``configure_views`` によって行われた最初のビュー設定命令を、オーバーライド
パッケージ内でオリジナルの設定関数をロードした後にオーバーライドする
ことができます:


.. code-block:: python
   :linenos:

   from pyramid.config import Configurator
   from originalapp import configure_views

   if __name == '__main__':
       config = Configurator()
       config.include(configure_views)
       config.add_view('theoverrideapp.views.theview', name='theview')


.. In this case, the ``theoriginalapp.views.theview`` view will never be
.. executed.  Instead, a new view, ``theoverrideapp.views.theview`` will be
.. executed instead, when request circumstances dictate.

この場合、 ``theoriginalapp.views.theview`` ビューは決して実行されません。
リクエストの状況にしたがってこのビューが実行される場合は、代わりに
新しいビュー ``theoverrideapp.views.theview`` が実行されます。


.. A similar pattern can be used to *extend* the application with ``add_view``
.. declarations.  Just register a new view against some other set of predicates
.. to make sure the URLs it implies are available on some other page rendering.

同様のパターンは ``add_view`` 宣言を持つアプリケーションを *拡張* する
ために使用できます。単に他の述語セットに対して新しいビューを登録して、
ビューに関係する URL がページレンダリングされるようにしてください。


.. index::
   pair: overriding; routes

.. Overriding Routes

.. _overriding_routes:

route のオーバーライド
~~~~~~~~~~~~~~~~~~~~~~

.. Route setup is currently typically performed in a sequence of ordered calls
.. to :meth:`~pyramid.config.Configurator.add_route`.  Because these calls are
.. ordered relative to each other, and because this ordering is typically
.. important, you should retain their relative ordering when performing an
.. override.  Typically, this means *copying* all the ``add_route`` statements
.. into the override package's file and changing them as necessary.  Then
.. disinclude any ``add_route`` statements from the original application.

route のセットアップは、現在典型的には
:meth:`~pyramid.config.Configurator.add_route` への順序付きの呼び出し
シーケンスによって行なわれます。これらの呼び出しがお互いに相対的で、
典型的にこの順序が重要なため、オーバーライドを行なう場合にはそれらの
相対的な順序を保持する必要があります。通常、これは ``add_route`` 命令を
オーバーライドパッケージのファイルにすべて *コピー* して、必要に応じて
それらを変更することを意味します。そして、オリジナルアプリケーションから
すべての ``add_route`` 命令のインクルードを取り除きます。


.. index::
   pair: overriding; assets

.. Overriding Assets

.. _overriding_resources:

asset のオーバーライド
~~~~~~~~~~~~~~~~~~~~~~

.. Assets are files on the filesystem that are accessible within a Python
.. *package*.  An entire chapter is devoted to assets: :ref:`assets_chapter`.
.. Within this chapter is a section named :ref:`overriding_assets_section`.
.. This section of that chapter describes in detail how to override package
.. assets with other assets by using the
.. :meth:`pyramid.config.Configurator.override_asset` method.  Add such
.. ``override_asset`` calls to your override package's ``__init__.py`` to
.. perform overrides.

asset はファイルシステム上にあるファイルで、 Python *パッケージ* 内で
アクセス可能なものです。完全に一章が asset の説明のために割かれています:
:ref:`assets_chapter` 。この章には :ref:`overriding_assets_section`
というタイトルの節があります。この節では、
:meth:`pyramid.config.Configurator.override_asset` メソッドを使用
することによってパッケージの asset を他の asset でオーバーライドする
方法について詳細に記述しています。オーバーライドを行なうために、
オーバーライドパッケージの ``__init__.py`` にそのような
``override_asset`` 呼び出しを追加してください。
