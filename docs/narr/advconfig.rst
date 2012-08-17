.. index::
   pair: advanced; configuration

.. Advanced Configuration

.. _advconfig_narr:

高度な設定
======================

.. To support application extensibility, the :app:`Pyramid`
.. :term:`Configurator`, by default, detects configuration conflicts and allows
.. you to include configuration imperatively from other packages or modules.  It
.. also, by default, performs configuration in two separate phases.  This allows
.. you to ignore relative configuration statement ordering in some
.. circumstances.

アプリケーションの拡張性をサポートするために、 :app:`Pyramid` の
:term:`Configurator` は、デフォルトで設定の衝突を検知し、他のパッケージ
やモジュールから設定を命令的に取り込むことを可能にします。さらにそれは
デフォルトで、分離された 2 つのフェーズで設定を行ないます。これは、いくつかの
状況で相対的な設定命令の順序をオーバーライドすることを可能にします。


.. index::
   pair: configuration; conflict detection

.. Conflict Detection

.. _conflict_detection:

衝突検知
------------------

.. Here's a familiar example of one of the simplest :app:`Pyramid` applications,
.. configured imperatively:

これは、おなじみの最も単純な :app:`Pyramid` アプリケーションの一例で、
命令的に設定されています:


.. code-block:: python
   :linenos:

   from wsgiref.simple_server import make_server
   from pyramid.config import Configurator
   from pyramid.response import Response

   def hello_world(request):
       return Response('Hello world!')

   if __name__ == '__main__':
       config = Configurator()
       config.add_view(hello_world)
       app = config.make_wsgi_app()
       server = make_server('0.0.0.0', 8080, app)
       server.serve_forever()


.. When you start this application, all will be OK.  However, what happens if we
.. try to add another view to the configuration with the same set of
.. :term:`predicate` arguments as one we've already added?

このアプリケーションを開始する時、すべては問題なく動くでしょう。
しかし、既に追加されているビューと同じ :term:`predicate` 引数のセットで
別のビューを設定に追加しようとした場合、何が起こるでしょうか。


.. code-block:: python
   :linenos:

   from wsgiref.simple_server import make_server
   from pyramid.config import Configurator
   from pyramid.response import Response

   def hello_world(request):
       return Response('Hello world!')

   def goodbye_world(request):
       return Response('Goodbye world!')

   if __name__ == '__main__':
       config = Configurator()

       config.add_view(hello_world, name='hello')

       # conflicting view configuration
       config.add_view(goodbye_world, name='hello')

       app = config.make_wsgi_app()
       server = make_server('0.0.0.0', 8080, app)
       server.serve_forever()


.. The application now has two conflicting view configuration statements.  When
.. we try to start it again, it won't start.  Instead, we'll receive a traceback
.. that ends something like this:

アプリケーションは今度は2つの衝突するビュー設定命令を持っています。
再びアプリケーションを開始しようとした時、それは開始しません。代わりに、
以下のような結果に終わるトレースバックを受け取るでしょう:


.. code-block:: guess
   :linenos:

   Traceback (most recent call last):
     File "app.py", line 12, in <module>
       app = config.make_wsgi_app()
     File "pyramid/config.py", line 839, in make_wsgi_app
       self.commit()
     File "pyramid/pyramid/config.py", line 473, in commit
       self._ctx.execute_actions()
     ... more code ...
   pyramid.exceptions.ConfigurationConflictError:
           Conflicting configuration actions
     For: ('view', None, '', None, <InterfaceClass pyramid.interfaces.IView>,
           None, None, None, None, None, False, None, None, None)
     Line 14 of file app.py in <module>: 'config.add_view(hello_world)'
     Line 17 of file app.py in <module>: 'config.add_view(goodbye_world)'


.. This traceback is trying to tell us:

このトレースバックは、以下のことを伝えようとしています:


.. - We've got conflicting information for a set of view configuration
..   statements (The ``For:`` line).

- ビュー設定命令に関する衝突情報を受け取っています (``For:`` の行)


.. - There are two statements which conflict, shown beneath the ``For:`` line:
..   ``config.add_view(hello_world. 'hello')`` on line 14 of ``app.py``, and
..   ``config.add_view(goodbye_world, 'hello')`` on line 17 of ``app.py``.

- 2つの衝突する文があり、 ``For:`` 行の下に示されています:
  ``app.py`` の 14 行目にある ``config.add_view(hello_world. 'hello')`` と、
  ``app.py`` の 17 行目にある ``config.add_view(goodbye_world, 'hello')`` 。


.. These two configuration statements are in conflict because we've tried to
.. tell the system that the set of :term:`predicate` values for both view
.. configurations are exactly the same.  Both the ``hello_world`` and
.. ``goodbye_world`` views are configured to respond under the same set of
.. circumstances.  This circumstance: the :term:`view name` (represented by the
.. ``name=`` predicate) is ``hello``.

システムに両方のビュー設定に対する :term:`predicate` 値が正確に同じであると
伝えようとしたので、これらの 2 つの設定命令は衝突した状態にあります。
``hello_world`` と ``goodbye_world`` の両方のビューが、同じ状況の下で
応答するように設定されています。その状況とは、 :term:`view name`
(``name=`` 述語によって表わされている) が ``hello`` であることです。


.. This presents an ambiguity that :app:`Pyramid` cannot resolve. Rather than
.. allowing the circumstance to go unreported, by default Pyramid raises a
.. :exc:`ConfigurationConflictError` error and prevents the application from
.. running.

これは :app:`Pyramid` が解決できない曖昧性を示しています。この状況を
報告せずに許可するのではなく、デフォルトで Pyramid は
:exc:`ConfigurationConflictError` エラーを上げて、アプリケーションの
作動を停止します。


.. Conflict detection happens for any kind of configuration: imperative
.. configuration or configuration that results from the execution of a
.. :term:`scan`.

衝突検知はあらゆる種類の設定に対して起こります: 命令的な設定、あるいは
:term:`scan` の実行に起因する設定。


.. Manually Resolving Conflicts

.. _manually_resolving_conflicts:

手動による衝突解決
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. There are a number of ways to manually resolve conflicts: by changing
.. registrations to not conflict, by strategically using
.. :meth:`pyramid.config.Configurator.commit`, or by using an "autocommitting"
.. configurator.

手動で衝突を解決する多くの方法があります: 衝突しないように登録を変更する
ことによって、 :meth:`pyramid.config.Configurator.commit` を戦略的に
使用することによって、または "autocommitting" configurator の使用によって。


.. The Right Thing

正しいやり方
+++++++++++++++

.. The most correct way to resolve conflicts is to "do the needful": change your
.. configuration code to not have conflicting configuration statements.  The
.. details of how this is done depends entirely on the configuration statements
.. made by your application.  Use the detail provided in the
.. :exc:`ConfigurationConflictError` to track down the offending conflicts and
.. modify your configuration code accordingly.

衝突を解決する最も正確な方法は「必要なことを行う」ことです: 衝突する
設定命令をなくすために設定コードを変更してください。これがどのように終わるかの
詳細は、完全にアプリケーションで行っている設定命令に依存します。
:exc:`ConfigurationConflictError` の中で提供されている詳細を使用して、
問題となっている衝突を特定して、それに従って設定コードを修正してください。


.. If you're getting a conflict while trying to extend an existing application,
.. and that application has a function which performs configuration like this
.. one:

既存のアプリケーションを拡張しようとする間に衝突が発生していて、その
アプリケーションには以下のような設定を行なう関数がある場合:


.. code-block:: python
   :linenos:

   def add_routes(config):
       config.add_route(...)


.. Don't call this function directly with ``config`` as an argument.  Instead,
.. use :meth:`pyramid.config.Configuration.include`:

``config`` を引数としてこの関数を直接呼び出さないでください。代わりに、
:meth:`pyramid.config.Configuration.include` を使用してください:


.. code-block:: python
   :linenos:

   config.include(add_routes)


.. Using :meth:`~pyramid.config.Configuration.include` instead of calling the
.. function directly provides a modicum of automated conflict resolution, with
.. the configuration statements you define in the calling code overriding those
.. of the included function.  See also :ref:`automatic_conflict_resolution` and
.. :ref:`including_configuration`.

関数を直接呼び出す代わりに :meth:`~pyramid.config.Configuration.include`
を使用すると、呼び出し側のコードに定義されている設定ステートメントが
include された関数の設定をオーバーライドするので、自動的な衝突の解決が
ある程度行われます。 :ref:`automatic_conflict_resolution` と
:ref:`including_configuration` も参照してください。


.. Using ``config.commit()``

``config.commit()`` の使用
++++++++++++++++++++++++++

.. You can manually commit a configuration by using the
.. :meth:`~pyramid.config.Configurator.commit` method between configuration
.. calls.  For example, we prevent conflicts from occurring in the application
.. we examined previously as the result of adding a ``commit``.  Here's the
.. application that generates conflicts:

設定呼び出しの間で :meth:`~pyramid.config.Configurator.commit` メソッド
を使用することによって、設定を手動でコミットすることができます。
例えば、 ``commit`` を加えた結果、以前に検討したアプリケーションで衝突が
発生することを防ぎます。これは、衝突を生成するアプリケーションです:


.. code-block:: python
   :linenos:

   from wsgiref.simple_server import make_server
   from pyramid.config import Configurator
   from pyramid.response import Response

   def hello_world(request):
       return Response('Hello world!')

   def goodbye_world(request):
       return Response('Goodbye world!')

   if __name__ == '__main__':
       config = Configurator()

       config.add_view(hello_world, name='hello')

       # conflicting view configuration
       config.add_view(goodbye_world, name='hello')

       app = config.make_wsgi_app()
       server = make_server('0.0.0.0', 8080, app)
       server.serve_forever()


.. We can prevent the two ``add_view`` calls from conflicting by issuing a call
.. to :meth:`~pyramid.config.Configurator.commit` between them:

2つの ``add_view`` 呼び出しの間に
:meth:`~pyramid.config.Configurator.commit` の呼び出しを行うことで、
それらの衝突を防ぐことができます:


.. code-block:: python
   :linenos:

   from wsgiref.simple_server import make_server
   from pyramid.config import Configurator
   from pyramid.response import Response

   def hello_world(request):
       return Response('Hello world!')

   def goodbye_world(request):
       return Response('Goodbye world!')

   if __name__ == '__main__':
       config = Configurator()

       config.add_view(hello_world, name='hello')

       config.commit() # commit any pending configuration actions

       # no-longer-conflicting view configuration
       config.add_view(goodbye_world, name='hello')

       app = config.make_wsgi_app()
       server = make_server('0.0.0.0', 8080, app)
       server.serve_forever()


.. In the above example we've issued a call to
.. :meth:`~pyramid.config.Configurator.commit` between the two ``add_view``
.. calls.  :meth:`~pyramid.config.Configurator.commit` will execute any pending
.. configuration statements.

上記の例において、2つの ``add_view`` 呼び出しの間で
:meth:`~pyramid.config.Configurator.commit` の呼び出しを行いました。
:meth:`~pyramid.config.Configurator.commit` はすべての待機中の
設定命令を実行します。


.. Calling :meth:`~pyramid.config.Configurator.commit` is safe at any time.  It
.. executes all pending configuration actions and leaves the configuration
.. action list "clean".

:meth:`~pyramid.config.Configurator.commit` の呼び出しは常に安全です。
それは、すべての待機中の設定アクションを実行して、設定アクションのリストを
「クリーン」にします。


.. Note that :meth:`~pyramid.config.Configurator.commit` has no effect when
.. you're using an *autocommitting* configurator (see
.. :ref:`autocommitting_configurator`).

*autocommitting* な configurator を使用している場合、
:meth:`~pyramid.config.Configurator.commit` の効果がないことに注意
してください (:ref:`autocommitting_configurator` を参照)。


.. Using An Autocommitting Configurator

.. _autocommitting_configurator:

autocommitting な Configurator の使用
+++++++++++++++++++++++++++++++++++++

.. You can also use a heavy hammer to circumvent conflict detection by using a
.. configurator constructor parameter: ``autocommit=True``.  For example:

configurator のコンストラクタにパラメーター *autocommit=True* を使用
することによって、衝突検知を回避するための重いハンマーを使用することも
できます。例えば:


.. code-block:: python
   :linenos:

   from pyramid.config import Configurator

   if __name__ == '__main__':
       config = Configurator(autocommit=True)


.. When the ``autocommit`` parameter passed to the Configurator is ``True``,
.. conflict detection (and :ref:`twophase_config`) is disabled.  Configuration
.. statements will be executed immediately, and succeeding statements will
.. override preceding ones.

Configurator に渡された ``autocommit`` パラメーターが ``True`` の場合、
衝突検知 (そして :ref:`twophase_config`) は無効になります。設定命令は直ちに
実行されるようになり、後続の設定が前のものをオーバーライドします。


.. :meth:`~pyramid.config.Configurator.commit` has no effect when ``autocommit``
.. is ``True``.

``autocommit`` が ``True`` の場合、
:meth:`~pyramid.config.Configurator.commit` は効果がありません。


.. If you use a Configurator in code that performs unit testing, it's usually a
.. good idea to use an autocommitting Configurator, because you are usually
.. unconcerned about conflict detection or two-phase configuration in test code.

ユニットテストを行なうコードの中で Configurator を使用する際、
通常テストコード中の衝突検知や2フェーズ設定に関心がないので、
autocommitting Configurator を使用することは通常よい考えです。


.. Automatic Conflict Resolution

.. _automatic_conflict_resolution:

自動的な衝突の解決
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. If your code uses the :meth:`~pyramid.config.Configurator.include` method to
.. include external configuration, some conflicts are automatically resolved.
.. Configuration statements that are made as the result of an "include" will be
.. overridden by configuration statements that happen within the caller of
.. the "include" method.

あなたのコードが :meth:`~pyramid.config.Configurator.include` メソッドを
使用して外部設定をインクルードする場合、いくつかの衝突は自動的に解決されます。
「インクルード」の結果作られる設定命令は、「インクルード」メソッドの
呼び出し元で起こる設定命令によってオーバーライドされます。


.. Automatic conflict resolution supports this goal: if a user wants to reuse a
.. Pyramid application, and they want to customize the configuration of this
.. application without hacking its code "from outside", they can "include" a
.. configuration function from the package and override only some of its
.. configuration statements within the code that does the include.  No conflicts
.. will be generated by configuration statements within the code which does the
.. including, even if configuration statements in the included code would
.. conflict if it was moved "up" to the calling code.

自動的な衝突の解決は以下のゴールをサポートします: Pyramid アプリケーション
を再利用したいと思ったユーザが、そのコードをハックせずに「外部から」
このアプリケーションの設定をカスタマイズしたければ、パッケージから
設定関数を「インクルード」して、その設定命令のうちのいくつかだけを
インクルードを行うコード内でオーバーライドできること。もしインクルード
されたコード中の設定命令を呼び出し元のコードまで移動させた場合に衝突が
起きるとしても、インクルードを行うコード内の設定命令によって衝突は生成
されません。


.. Methods Which Provide Conflict Detection

衝突検知を提供しているメソッド
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. These are the methods of the configurator which provide conflict detection:

これらは衝突検知を提供する configurator のメソッドです:


.. :meth:`~pyramid.config.Configurator.add_view`,
.. :meth:`~pyramid.config.Configurator.add_route`,
.. :meth:`~pyramid.config.Configurator.add_renderer`,
.. :meth:`~pyramid.config.Configurator.set_request_factory`,
.. :meth:`~pyramid.config.Configurator.set_session_factory`,
.. :meth:`~pyramid.config.Configurator.set_request_property`,
.. :meth:`~pyramid.config.Configurator.set_root_factory`,
.. :meth:`~pyramid.config.Configurator.set_view_mapper`,
.. :meth:`~pyramid.config.Configurator.set_authentication_policy`,
.. :meth:`~pyramid.config.Configurator.set_authorization_policy`,
.. :meth:`~pyramid.config.Configurator.set_renderer_globals_factory`,
.. :meth:`~pyramid.config.Configurator.set_locale_negotiator`,
.. :meth:`~pyramid.config.Configurator.set_default_permission`,
.. :meth:`~pyramid.config.Configurator.add_traverser`,
.. :meth:`~pyramid.config.Configurator.add_resource_url_adapter`,
.. and :meth:`~pyramid.config.Configurator.add_response_adapter`.

:meth:`~pyramid.config.Configurator.add_view`,
:meth:`~pyramid.config.Configurator.add_route`,
:meth:`~pyramid.config.Configurator.add_renderer`,
:meth:`~pyramid.config.Configurator.set_request_factory`,
:meth:`~pyramid.config.Configurator.set_session_factory`,
:meth:`~pyramid.config.Configurator.set_request_property`,
:meth:`~pyramid.config.Configurator.set_root_factory`,
:meth:`~pyramid.config.Configurator.set_view_mapper`,
:meth:`~pyramid.config.Configurator.set_authentication_policy`,
:meth:`~pyramid.config.Configurator.set_authorization_policy`,
:meth:`~pyramid.config.Configurator.set_renderer_globals_factory`,
:meth:`~pyramid.config.Configurator.set_locale_negotiator`,
:meth:`~pyramid.config.Configurator.set_default_permission`,
:meth:`~pyramid.config.Configurator.add_traverser`,
:meth:`~pyramid.config.Configurator.add_resource_url_adapter`,
:meth:`~pyramid.config.Configurator.add_response_adapter`.


.. :meth:`~pyramid.config.Configurator.add_static_view` also indirectly
.. provides conflict detection, because it's implemented in terms of the
.. conflict-aware ``add_route`` and ``add_view`` methods.

:meth:`~pyramid.config.Configurator.add_static_view` は、衝突を考慮する
``add_route`` および ``add_view`` メソッドによって実装されているため、
さらに間接的な衝突検知を提供します。


.. index::
   pair: configuration; including from external sources

.. Including Configuration from External Sources

.. _including_configuration:

外部ソースからの設定インクルード
---------------------------------------------

.. Some application programmers will factor their configuration code in such a
.. way that it is easy to reuse and override configuration statements.  For
.. example, such a developer might factor out a function used to add routes to
.. his application:

一部のアプリケーションプログラマは、設定命令を再利用およびオーバーライド
することが簡単であるのと同じ方法で設定コードを分解するでしょう。
例えば、そのような開発者は、 route をアプリケーションに追加するために
使用される関数を分解するかもしれません:


.. code-block:: python
   :linenos:

   def add_routes(config):
       config.add_route(...)


.. Rather than calling this function directly with ``config`` as an argument.
.. Instead, use :meth:`pyramid.config.Configuration.include`:

``config`` を引数としてこの関数を直接呼ぶのではなく。
代わりに、 :meth:`pyramid.config.Configuration.include` を使用します:


.. code-block:: python
   :linenos:

   config.include(add_routes)


.. Using ``include`` rather than calling the function directly will allow
.. :ref:`automatic_conflict_resolution` to work.

関数を直接呼ぶのではなく ``include`` を使用することで、
:ref:`automatic_conflict_resolution` が働くようになるでしょう。


.. :meth:`~pyramid.config.Configuration.include` can also accept a :term:`module`
.. as an argument:

:meth:`~pyramid.config.Configuration.include` は、引数として :term:`module`
を受け取ることもできます:


.. code-block:: python
   :linenos:

   import myapp

   config.include(myapp)


.. For this to work properly, the ``myapp`` module must contain a callable with
.. the special name ``includeme``, which should perform configuration (like the
.. ``add_routes`` callable we showed above as an example).

これが適切に動作するためには、 ``myapp`` モジュールに特殊名 ``includeme``
を持つ callable が含まれている必要があります。それは (例として上に示した
``add_routes`` callable と同じように) 設定を行ないます。


.. :meth:`~pyramid.config.Configuration.include` can also accept a :term:`dotted
.. Python name` to a function or a module.

:meth:`~pyramid.config.Configuration.include` はさらに、関数または
モジュールに対する :term:`dotted Python name` を受け取ることもできます。


.. .. note: See :ref:`the_include_tag` for a declarative alternative to
..    the :meth:`~pyramid.config.Configurator.include` method.

.. 注: :meth:`~pyramid.config.Configurator.include` メソッドの宣言的な
   代替物については、 :ref:`the_include_tag` を参照してください。


.. Two-Phase Configuration

.. _twophase_config:

2フェーズ設定
-----------------------

.. When a non-autocommitting :term:`Configurator` is used to do configuration
.. (the default), configuration execution happens in two phases.  In the first
.. phase, "eager" configuration actions (actions that must happen before all
.. others, such as registering a renderer) are executed, and *discriminators*
.. are computed for each of the actions that depend on the result of the eager
.. actions.  In the second phase, the discriminators of all actions are compared
.. to do conflict detection.

設定を行うために autocommitting でない :term:`Configurator` が使用される
場合 (デフォルト)、設定の実行は2つのフェーズで起こります。第1フェーズでは、
"eager" 設定アクション (レンダラーの登録のような、他のものよりも先に
起こる必要のあるアクション) が実行されます。そして、 eager アクションの
結果に依存する各々のアクションのために *識別子* (discriminator) が計算
されます。第2フェーズでは、衝突検知を行うためにすべてのアクションの
識別子が比較されます。


.. Due to this, for configuration methods that have no internal ordering
.. constraints, execution order of configuration method calls is not important.
.. For example, the relative ordering of
.. :meth:`~pyramid.config.Configurator.add_view` and
.. :meth:`~pyramid.config.Configurator.add_renderer` is unimportant when a
.. non-autocommitting configurator is used.  This code snippet:

これにより、内部的な順序の制約がない設定メソッドについては、設定メソッド
呼び出しの実行順序は重要ではありません。例えば、 autocommitting でない
configurator が使用される場合、
:meth:`~pyramid.config.Configurator.add_view` と
:meth:`~pyramid.config.Configurator.add_renderer` の相対順序は重要では
ありません。このコードスニペットは:


.. code-block:: python
   :linenos:

   config.add_view('some.view', renderer='path_to_custom/renderer.rn')
   config.add_renderer('.rn', SomeCustomRendererFactory)


.. Has the same result as:

これと同じ結果になります:


.. code-block:: python
   :linenos:

   config.add_renderer('.rn', SomeCustomRendererFactory)
   config.add_view('some.view', renderer='path_to_custom/renderer.rn')


.. Even though the view statement depends on the registration of a custom
.. renderer, due to two-phase configuration, the order in which the
.. configuration statements are issued is not important.  ``add_view`` will be
.. able to find the ``.rn`` renderer even if ``add_renderer`` is called after
.. ``add_view``.

たとえビュー命令がカスタムレンダラーの登録に依存しても、
2フェーズの設定により、設定命令が実行される順序は重要ではありません。
``add_renderer`` が ``add_view`` の後で呼ばれても、
``add_view`` は ``.rn`` レンダラーを見つけることができるでしょう。


.. The same is untrue when you use an *autocommitting* configurator (see
.. :ref:`autocommitting_configurator`).  When an autocommitting configurator is
.. used, two-phase configuration is disabled, and configuration statements must
.. be ordered in dependency order.

*autocommitting* な configurator を使用する場合、同じことは真ではありません
(:ref:`autocommitting_configurator` を参照)。 autocommitting な
configurator が使用されている場合、 2フェーズの設定は無効になります。
また、設定命令は依存関係の順番で並べられなければなりません。


.. Some configuration methods, such as
.. :meth:`~pyramid.config.Configurator.add_route` have internal ordering
.. constraints: the routes they imply require relative ordering.  Such ordering
.. constraints are not absolved by two-phase configuration.  Routes are still
.. added in configuration execution order.

:meth:`~pyramid.config.Configurator.add_route` のようないくつかの設定
メソッドには内部的な順序の制約があります: それらが関係する route には
相対順序が必要です。そのような順序制約は、2フェーズの設定によって免除
されません。 route は、依然として設定実行順に追加されます。
