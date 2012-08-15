.. index::
   single: extending configuration

.. Extending Pyramid Configuration

.. _extconfig_narr:

Pyramid Configurator の拡張
===============================

.. Pyramid allows you to extend its Configurator with custom directives.  Custom
.. directives can use other directives, they can add a custom :term:`action`,
.. they can participate in :term:`conflict resolution`, and they can provide
.. some number of :term:`introspectable` objects.

Pyramid では、カスタムディレクティブによって Configurator を拡張することが
できます。カスタムディレクティブは、他のディレクティブを使用したり、
カスタム :term:`action` を追加したり、 :term:`conflict resolution` に参加
したり、 :term:`introspectable` オブジェクトを提供したりすることができます。


.. index::
   single: add_directive
   pair: configurator; adding directives

.. Adding Methods to the Configurator via ``add_directive``

.. _add_directive:

``add_directive`` による Configurator へのメソッドの追加
--------------------------------------------------------

.. Framework extension writers can add arbitrary methods to a
.. :term:`Configurator` by using the
.. :meth:`pyramid.config.Configurator.add_directive` method of the configurator.
.. Using :meth:`~pyramid.config.Configurator.add_directive` makes it possible to
.. extend a Pyramid configurator in arbitrary ways, and allows it to perform
.. application-specific tasks more succinctly.

フレームワーク拡張の作者は configurator の
:meth:`pyramid.config.Configurator.add_directive` メソッドを使用することで
:term:`Configurator` に任意のメソッドを追加することができます。
:meth:`~pyramid.config.Configurator.add_directive` を使用することによって、
Pyramid configurator を任意の方法で拡張することが可能になり、
アプリケーション特有のタスクをより簡潔に行なえるようになります。


.. The :meth:`~pyramid.config.Configurator.add_directive` method accepts two
.. positional arguments: a method name and a callable object.  The callable
.. object is usually a function that takes the configurator instance as its
.. first argument and accepts other arbitrary positional and keyword arguments.
.. For example:

:meth:`~pyramid.config.Configurator.add_directive` メソッドは2つの
位置引数を受け取ります: メソッド名および callable オブジェクトです。
callable オブジェクトは、通常その最初の引数として configurator
インスタンスを取る関数で、他の任意の位置引数とキーワード引数を
受け取ります。例えば:


.. code-block:: python
   :linenos:

   from pyramid.events import NewRequest
   from pyramid.config import Configurator

   def add_newrequest_subscriber(config, subscriber):
       config.add_subscriber(subscriber, NewRequest)

   if __name__ == '__main__':
       config = Configurator()
       config.add_directive('add_newrequest_subscriber',
                            add_newrequest_subscriber)


.. Once :meth:`~pyramid.config.Configurator.add_directive` is called, a user can
.. then call the added directive by its given name as if it were a built-in
.. method of the Configurator:

一旦 :meth:`~pyramid.config.Configurator.add_directive` が呼ばれると、
ユーザはその後、あたかもそれが configurator のビルトインメソッドで
あるかのように、追加されたディレクティブをその名前で呼ぶことができます:


.. code-block:: python
   :linenos:

   def mysubscriber(event):
       print event.request

   config.add_newrequest_subscriber(mysubscriber)


.. A call to :meth:`~pyramid.config.Configurator.add_directive` is often
.. "hidden" within an ``includeme`` function within a "frameworky" package meant
.. to be included as per :ref:`including_configuration` via
.. :meth:`~pyramid.config.Configurator.include`.  For example, if you put this
.. code in a package named ``pyramid_subscriberhelpers``:

:meth:`~pyramid.config.Configurator.add_directive` の呼び出しは、
:meth:`~pyramid.config.Configurator.include` による
:ref:`including_configuration` によってインクルードされることを意図して、
しばしば「フレームワーク風」パッケージの ``includeme`` 関数内に
「隠蔽」されます。例えば、 ``pyramid_subscriberhelpers`` という名前の
パッケージに以下のコードを入れた場合:


.. code-block:: python
   :linenos:

   def includeme(config):
       config.add_directive('add_newrequest_subscriber',
                            add_newrequest_subscriber)


.. The user of the add-on package ``pyramid_subscriberhelpers`` would then be
.. able to install it and subsequently do:

アドオンパッケージ ``pyramid_subscriberhelpers`` のユーザは、その後
それをインストールして、続いて以下のようにすることができるでしょう:


.. code-block:: python
   :linenos:

   def mysubscriber(event):
       print event.request

   from pyramid.config import Configurator
   config = Configurator()
   config.include('pyramid_subscriberhelpers')
   config.add_newrequest_subscriber(mysubscriber)


.. Using ``config.action`` in a Directive

ディレクティブ内での ``config.action`` の使用
---------------------------------------------

.. If a custom directive can't do its work exclusively in terms of existing
.. configurator methods (such as
.. :meth:`pyramid.config.Configurator.add_subscriber`, as above), the directive
.. may need to make use of the :meth:`pyramid.config.Configurator.action`
.. method.  This method adds an entry to the list of "actions" that Pyramid will
.. attempt to process when :meth:`pyramid.config.Configurator.commit` is called.
.. An action is simply a dictionary that includes a :term:`discriminator`,
.. possibly a callback function, and possibly other metadata used by Pyramid's
.. action system.

カスタムディレクティブが (上記の
:meth:`~pyramid.config.Configurator.add_subscriber` のように) 既存の
configurator メソッドだけで仕事を行うことができない場合、
そのディレクティブは :meth:`pyramid.config.Configurator.action` メソッドを
利用する必要があるかもしれません。このメソッドは、
:meth:`pyramid.config.Configurator.commit` が呼ばれたときに
Pyramid が処理を試みる「アクション」のリストにエントリを追加します。
アクションは、 *識別子* (discriminator) と、任意のコールバック関数と、
Pyramid のアクションシステムによって使用される他の任意のメタデータを含む
単なる辞書です。


.. Here's an example directive which uses the "action" method:

これは「アクション」メソッドを使用するディレクティブの例です:


.. code-block:: python
   :linenos:

   def add_jammyjam(config, jammyjam):
       def register():
           config.registry.jammyjam = jammyjam
       config.action('jammyjam', register)

   if __name__ == '__main__':
       config = Configurator()
       config.add_directive('add_jammyjam', add_jammyjam)


.. Fancy, but what does it do?  The action method accepts a number of arguments.
.. In the above directive named ``add_jammyjam``, we call
.. :meth:`~pyramid.config.Configurator.action` with two arguments: the string
.. ``jammyjam`` is passed as the first argument named ``discriminator``, and the
.. closure function named ``register`` is passed as the second argument named
.. ``callable``.

なかなか手が込んでいますが、これは何を行うのでしょうか。アクションメソッド
はいくつかの引数を受け取ります。上記の ``add_jammyjam`` という名前の
ディレクティブでは、 :meth:`pyramid.config.Configurator.action` を 2 つの
引数で呼び出しています: 文字列の ``jammyjam`` は ``discriminator`` という名前の
最初の引数として渡されます。また、 ``register`` という名前のクロージャー
関数は ``callable`` という名前の 2 番目の引数として渡されます。


.. When the :meth:`~pyramid.config.Configurator.action` method is called, it
.. appends an action to the list of pending configuration actions.  All pending
.. actions with the same discriminator value are potentially in conflict with
.. one another (see :ref:`conflict_detection`).  When the
.. :meth:`~pyramid.config.Configurator.commit` method of the Configurator is
.. called (either explicitly or as the result of calling
.. :meth:`~pyramid.config.Configurator.make_wsgi_app`), conflicting actions are
.. potentially automatically resolved as per
.. :ref:`automatic_conflict_resolution`.  If a conflict cannot be automatically
.. resolved, a :exc:`ConfigurationConflictError` is raised and application
.. startup is prevented.

:meth:`~pyramid.config.Configurator.action` メソッドが呼ばれる場合、
それは待機中の設定アクションのリストにアクションを追加します。
同じ識別子の値を持つすべての待機中のアクションは、潜在的に
お互い衝突する可能性があります (:ref:`conflict_detection` を参照)。
Configurator の :meth:`~pyramid.config.Configurator.commit` メソッドが
呼ばれた時 (明示的に、または
:meth:`~pyramid.config.Configurator.make_wsgi_app` を呼んだ結果として)、
衝突するアクションは :ref:`automatic_conflict_resolution` を通して
自動的に解決される可能性があります。自動的に衝突を解決することができない
場合、 :exc:`ConfigurationConflictError` 例外が発生し、アプリケーション
のスタートアップが停止されます。


.. In our above example, therefore, if a consumer of our ``add_jammyjam``
.. directive did this:

したがって上記の例において ``add_jammyjam`` ディレクティブのユーザが
このようにしたなら:


.. code-block:: python

   config.add_jammyjam('first')
   config.add_jammyjam('second')


.. When the action list was committed resulting from the set of calls above, our
.. user's application would not start, because the discriminators of the actions
.. generated by the two calls are in direct conflict.  Automatic conflict
.. resolution cannot resolve the conflict (because no ``config.include`` is
.. involved), and the user provided no intermediate
.. :meth:`pyramid.config.Configurator.commit` call between the calls to
.. ``add_jammyjam`` to ensure that the successive calls did not conflict with
.. each other.

上記の一連の呼び出しに起因してアクションリストがコミットされた時、
ユーザのアプリケーションは開始しません。2つの呼び出しによって生成された
アクションの識別子が直接の衝突状態にあるからです。自動的な衝突の解決は
この衝突を解決することができず (``config.include`` が使われていないので) 、
ユーザは ``add_jammyjam`` の呼び出しの間で連続する呼び出しが互いと衝突
しないことを保証するために中間の
:meth:`pyramid.config.Configurator.commit` 呼び出しを提供していません。


.. This demonstrates the purpose of the discriminator argument to the action
.. method: it's used to indicate a uniqueness constraint for an action.  Two
.. actions with the same discriminator will conflict unless the conflict is
.. automatically or manually resolved. A discriminator can be any hashable
.. object, but it is generally a string or a tuple.  *You use a discriminator to
.. declaratively ensure that the user doesn't provide ambiguous configuration
.. statements.*

これはアクションメソッドに対する識別子引数の目的を実証しています: それは
アクションのユニーク制約を示すために使用されます。同じ識別子を持つ 2 つの
アクションは、衝突が自動的にあるいは手動で解決されない限り衝突するでしょう。
識別子は任意のハッシュ可能オブジェクトにすることができますが、一般的には
文字列またはタプルです。 *ユーザが曖昧な設定命令を提供しないことを
宣言的に保証するために識別子を使用してください。*


.. But let's imagine that a consumer of ``add_jammyjam`` used it in such a way
.. that no configuration conflicts are generated.

しかし、 ``add_jammyjam`` のユーザが設定の衝突が起きない以下のような方法で
使用した場合を考えてみましょう。


.. code-block:: python

   config.add_jammyjam('first')


.. What happens now?  When the ``add_jammyjam`` method is called, an action is
.. appended to the pending actions list.  When the pending configuration actions
.. are processed during :meth:`~pyramid.config.Configurator.commit`, and no
.. conflicts occur, the *callable* provided as the second argument to the
.. :meth:`~pyramid.config.Configurator.action` method within ``add_jammyjam`` is
.. called with no arguments.  The callable in ``add_jammyjam`` is the
.. ``register`` closure function.  It simply sets the value
.. ``config.registry.jammyjam`` to whatever the user passed in as the
.. ``jammyjam`` argument to the ``add_jammyjam`` function.  Therefore, the
.. result of the user's call to our directive will set the ``jammyjam``
.. attribute of the registry to the string ``first``.  *A callable is used by a
.. directive to defer the result of a user's call to the directive until
.. conflict detection has had a chance to do its job*.

今度は何が起こるでしょうか。 ``add_jammyjam`` メソッドが呼ばれた時、
待機中のアクションリストにアクションが追加されます。
:meth:`~pyramid.config.Configurator.commit` によって待機中の設定
アクションが処理され、衝突が生じない場合、 ``add_jammyjam`` 内の
:meth:`~pyramid.config.Configurator.action` メソッドの第 2 引数として
渡された *callable* が引数なしで呼ばれます。 ``add_jammyjam`` 内の
callable は、 ``register`` クロージャー関数です。それは、単純にユーザが
``add_jammyjam`` 関数に ``jammyjam`` 引数として渡したものをなんでも
値 ``config.registry.jammyjam`` に設定します。したがって、ユーザが
ディレクティブを呼び出した結果として、レジストリの ``jammyjam`` 属性に
文字列 ``first`` が設定されるでしょう。 callable は、衝突検知が働く
ようになるまでユーザがディレクティブを呼び出した結果を遅延するために、
ディレクティブによって使用されます。


.. Other arguments exist to the :meth:`~pyramid.config.Configurator.action`
.. method, including ``args``, ``kw``, ``order``, and ``introspectables``.  

:meth:`~pyramid.config.Configurator.action` メソッドには他に ``args``,
``kw``, ``order``, ``introspectables`` といった引数が存在します。


.. ``args`` and ``kw`` exist as values, which, if passed, will be used as
.. arguments to the ``callable`` function when it is called back.  For example
.. our directive might use them like so:

``args`` と ``kw`` は、もし渡されれば、 ``callable`` 関数が呼ばれるときの
引数として使用される値として存在します。例えば、ディレクティブはそれらを
以下のように使用するかもしれません:


.. code-block:: python
   :linenos:

   def add_jammyjam(config, jammyjam):
       def register(*arg, **kw):
           config.registry.jammyjam_args = arg
           config.registry.jammyjam_kw = kw
           config.registry.jammyjam = jammyjam
       config.action('jammyjam', register, args=('one',), kw={'two':'two'})


.. In the above example, when this directive is used to generate an action, and
.. that action is committed, ``config.registry.jammyjam_args`` will be set to
.. ``('one',)`` and ``config.registry.jammyjam_kw`` will be set to
.. ``{'two':'two'}``.  ``args`` and ``kw`` are honestly not very useful when
.. your ``callable`` is a closure function, because you already usually have
.. access to every local in the directive without needing them to be passed
.. back.  They can be useful, however, if you don't use a closure as a callable.

上記の例において、このディレクティブがアクションを生成するために使用され、
そのアクションがコミットされる時、
``config.registry.jammyjam_args`` は ``('one',)`` に設定されて、
``config.registry.jammyjam_kw`` は ``{'two':'two'}`` に設定されるでしょう。
正直なところ、 ``callable`` がクロージャー関数の場合 ``args`` と ``kw``
はあまり有用ではありません。なぜなら、それらを渡すまでもなくディレクティブ
内のすべてのローカル変数に普通にアクセスできるからです。しかし、 callable
としてクロージャーを使用していなければ、それらは有用なことがあります。


.. ``order`` is a crude order control mechanism.  ``order`` defaults to the
.. integer ``0``; it can be set to any other integer.  All actions that share an
.. order will be called before other actions that share a higher order.  This
.. makes it possible to write a directive with callable logic that relies on the
.. execution of the callable of another directive being done first.  For
.. example, Pyramid's :meth:`pyramid.config.Configurator.add_view` directive
.. registers an action with a higher order than the
.. :meth:`pyramid.config.Configurator.add_route` method.  Due to this, the
.. ``add_view`` method's callable can assume that, if a ``route_name`` was
.. passed to it, that a route by this name was already registered by
.. ``add_route``, and if such a route has not already been registered, it's a
.. configuration error (a view that names a nonexistent route via its
.. ``route_name`` parameter will never be called).

``order`` は大雑把な順序管理メカニズムです。 ``order`` のデフォルトは
整数 ``0`` です; それは他の任意の整数にセットすることができます。 order
を共有するすべてのアクションは、より高い order を共有する他のアクション
より前に呼ばれるでしょう。これは、別のディレクティブの callable が先に
実行されていることに依存するcallable ロジックを持つディレクティブを書く
ことを可能にします。例えば、 Pyramid の
:meth:`pyramid.config.Configurator.add_view` ディレクティブは
:meth:`pyramid.config.Configurator.add_route` メソッドより高い order で
アクションを登録します。これにより、 ``add_view`` メソッドの callable は、
``route_name`` が渡された場合、この名前による route は ``add_route``
によって既に登録されていると仮定することができ、またそのような route が
まだ登録されていない場合は設定エラーとすることができます (``route_name``
パラメーターに存在しない route を指定されたビューは決して呼ばれません)。


.. ``introspectables`` is a sequence of :term:`introspectable` objects.  You can
.. pass a sequence of introspectables to the
.. :meth:`~pyramid.config.Configurator.action` method, which allows you to
.. augment Pyramid's configuration introspection system.

``introspectables`` は ``introspectable`` オブジェクトのシーケンスです。
``introspectables`` のシーケンスを
:meth:`~pyramid.config.Configurator.action` メソッドに渡すことができ、
それにより Pyramid の設定 introspection システムを拡張することが可能です。


.. Adding Configuration Introspection

.. _introspection:

Configuration introspection の追加
----------------------------------

.. note::

   .. The introspection subsystem is new in Pyramid 1.3.

   introspection サブシステムは Pyramid 1.3 からの新機能です。


.. Pyramid provides a configuration introspection system that can be used by
.. debugging tools to provide visibility into the configuration of a running
.. application.

Pyramid は、デバッグ用ツールが実行中のアプリケーションの設定を見ることの
できる、設定 introspection システムを提供します。


.. All built-in Pyramid directives (such as
.. :meth:`pyramid.config.Configurator.add_view` and
.. :meth:`pyramid.config.Configurator.add_route`) register a set of
.. introspectables when called.  For example, when you register a view via
.. ``add_view``, the directive registers at least one introspectable: an
.. introspectable about the view registration itself, providing human-consumable
.. values for the arguments it was passed.  You can later use the introspection
.. query system to determine whether a particular view uses a renderer, or
.. whether a particular view is limited to a particular request method, or which
.. routes a particular view is registered against.  The Pyramid "debug toolbar"
.. makes use of the introspection system in various ways to display information
.. to Pyramid developers.

すべてのビルトイン Pyramid ディレクティブ
(:meth:`pyramid.config.Configurator.add_view` や
:meth:`pyramid.config.Configurator.add_route` など) は、呼び出された時に
いくつかの introspectable を登録します。例えば、 ``add_view`` によって
ビューを登録する場合、このディレクティブは少なくとも 1 つの
introspectable を登録します: ビュー登録自体に関する introspectable です。
それは渡された引数に対して人間が判読可能な値を提供します。特定のビュー
がレンダラーを使用するかどうか、特定のビューが特定のリクエストメソッドに制限
されているかどうか、あるいは、特定のビューがどの route に対して登録されて
いるかを判断するために、後で introspection 質問システムを使用することができます。
Pyramid 「デバッグツールバー」は、 Pyramid の開発者に情報を表示するために
様々な方法で introspection システムを利用します。


.. Introspection values are set when a sequence of :term:`introspectable`
.. objects is passed to the :meth:`~pyramid.config.Configurator.action` method.
.. Here's an example of a directive which uses introspectables:

:term:`introspectable` オブジェクトのシーケンスが
:meth:`~pyramid.config.Configurator.action` メソッドに渡される場合、
introspection 値がセットされます。これは、 introspectable を使用するディレクティブの
一例です:


.. code-block:: python
   :linenos:

   def add_jammyjam(config, value):
       def register():
           config.registry.jammyjam = value
       intr = config.introspectable(category_name='jammyjams', 
                                    discriminator='jammyjam',
                                    title='a jammyjam',
                                    type_name=None)
       intr['value'] = value
       config.action('jammyjam', register, introspectables=(intr,))

   if __name__ == '__main__':
       config = Configurator()
       config.add_directive('add_jammyjam', add_jammyjam)


.. If you notice, the above directive uses the ``introspectable`` attribute of a
.. Configurator (:attr:`pyramid.config.Configurator.introspectable`) to create
.. an introspectable object.  The introspectable object's constructor requires
.. at least four arguments: the ``category_name``, the ``discriminator``, the
.. ``title``, and the ``type_name``.

気づいたかもしれませんが、上記のディレクティブは introspectable オブジェクトを
作成するために Configurator の ``introspectable`` 属性
(:attr:`pyramid.config.Configurator.introspectable`)
を使用しています。 introspectable オブジェクトのコンストラクタは、
少なくとも4つの引数を要求します:
``category_name``, ``discriminator``, ``title``, ``type_name`` です。


.. The ``category_name`` is a string representing the logical category for this
.. introspectable.  Usually the category_name is a pluralization of the type of
.. object being added via the action.

``category_name`` はこの introspectable の論理的なカテゴリを表わす文字列です。
通常 category_name は、アクションによって加えられているオブジェクトの型の
複数形です。


.. The ``discriminator`` is a value unique **within the category** (unlike the
.. action discriminator, which must be unique within the entire set of actions).
.. It is typically a string or tuple representing the values unique to this
.. introspectable within the category.  It is used to generate links and as part
.. of a relationship-forming target for other introspectables.

``discriminator`` は、 **カテゴリ内で** ユニークな値です (これはアクション
の集合全体でユニークでなければならないアクション識別子とは異なります)。
それは典型的に、このカテゴリ内でこの introspectable に対してユニークな
値を表わす文字列またはタプルです。それはリンクを生成するのに使用され、
他の introspectable に対してリレーションを構成するターゲットの一部として
使用されます。


.. The ``title`` is a human-consumable string that can be used by introspection
.. system frontends to show a friendly summary of this introspectable.

``title`` は人間が判読可能な文字列で、この introspectable の分かりやすい
要約を表示するために introspection システムのフロントエンドによって使用されます。


.. The ``type_name`` is a value that can be used to subtype this introspectable
.. within its category for for sorting and presentation purposes.  It can be any
.. value.

``type_name`` は、ソートと表示を目的としてこの introspectable をカテゴリ
内で下位分類するために使用される値です。それは任意の値にすることができます。


.. An introspectable is also dictionary-like.  It can contain any set of
.. key/value pairs, typically related to the arguments passed to its related
.. directive.  While the category_name, discriminator, title and type_name are
.. *metadata* about the introspectable, the values provided as key/value pairs
.. are the actual data provided by the introspectable.  In the above example, we
.. set the ``value`` key to the value of the ``value`` argument passed to the
.. directive.

introspectable は辞書のようにアクセスすることもできます。任意のキー/値
のペアを格納することができますが、典型的には関連するディレクティブに
渡された引数に関連した値が格納されます。 category_name, discriminator,
title および type_name は introspectable に関する *メタデータ* です。
一方、キー/値ペアとして提供される値はintrospectable によって提供される
実際のデータです。上記の例では、ディレクティブに渡された ``value`` 引数の
値を ``value`` キーに設定しています。


.. Our directive above mutates the introspectable, and passes it in to the
.. ``action`` method as the first element of a tuple as the value of the
.. ``introspectable`` keyword argument.  This associates this introspectable
.. with the action.  Introspection tools will then display this introspectable
.. in their index.

上記のディレクティブは、 introspectable を変更して、それを
``introspectable`` キーワード引数に渡すタプルの最初の要素として
``action`` メソッドに渡します。これにより、この introspectable は
このアクションと関連付けられます。これ以降、 introspection ツールの
一覧にこの introspectable が表示されるようになります。


.. Introspectable Relationships

introspectable の関連付け
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Two introspectables may have relationships between each other.

2 つの introspectable は互いに関連を持つことができます。


.. code-block:: python
   :linenos:

   def add_jammyjam(config, value, template):
       def register():
           config.registry.jammyjam = (value, template)
       intr = config.introspectable(category_name='jammyjams', 
                                    discriminator='jammyjam',
                                    title='a jammyjam',
                                    type_name=None)
       intr['value'] = value
       tmpl_intr = config.introspectable(category_name='jammyjam templates',
                                         discriminator=template,
                                         title=template,
                                         type_name=None)
       tmpl_intr['value'] = template
       intr.relate('jammyjam templates', template)
       config.action('jammyjam', register, introspectables=(intr, tmpl_intr))

   if __name__ == '__main__':
       config = Configurator()
       config.add_directive('add_jammyjam', add_jammyjam)


.. In the above example, the ``add_jammyjam`` directive registers two
.. introspectables.  The first is related to the ``value`` passed to the
.. directive; the second is related to the ``template`` passed to the directive.
.. If you believe a concept within a directive is important enough to have its
.. own introspectable, you can cause the same directive to register more than
.. one introspectable, registering one introspectable for the "main idea" and
.. another for a related concept.

上記の例で、 ``add_jammyjam`` ディレクティブは 2 つの introspectable
を登録しています。
1 つ目はディレクティブに渡された ``value`` と関係しています;
2 つ目はディレクティブに渡された ``template`` と関係しています。
ディレクティブ内の概念がそれ自身の introspectable を持っても良いくらいに
重要だと考えるなら、 1 つのディレクティブに対して複数の
introspectable を (「主要な概念」に対して 1 つの introspectable を、
関連する概念に対して別の introspectable を) 登録することができます。


.. The call to ``intr.relate`` above
.. (:meth:`pyramid.interfaces.IIntrospectable.relate`) is passed two arguments:
.. a category name and a directive.  The example above effectively indicates
.. that the directive wishes to form a relationship between the ``intr``
.. introspectable and the ``tmpl_intr`` introspectable; the arguments passed to
.. ``relate`` are the category name and discriminator of the ``tmpl_intr``
.. introspectable.

上記の ``intr.relate`` (:meth:`pyramid.interfaces.IIntrospectable.relate`)
の呼び出しにはカテゴリ名とディレクティブの 2 つの引数が渡されています:
上記の例は、実質的にディレクティブが ``intr`` introspectable と
``tmpl_intr`` introspectable の関係を築く意思があることを示しています;
``relate`` に渡された引数は、カテゴリ名および ``tmpl_intr`` introspectable
の識別子です。


.. Relationships need not be made between two introspectables created by the
.. same directive.  Instead, a relationship can be formed between an
.. introspectable created in one directive and another introspectable created in
.. another by calling ``relate`` on either side with the other directive's
.. category name and discriminator.  An error will be raised at configuration
.. commit time if you attempt to relate an introspectable with another
.. nonexistent introspectable, however.

同じディレクティブによって作られた 2 つの introspectable の間で関係を
作る必要はありません。代わりに、あるディレクティブによって作成された
introspectable と別のディレクティブによって作成された別の
introspectable の間で、片方の側でもう片方のディレクティブのカテゴリ名と
識別子を伴って ``relate`` を呼ぶことによって関係を築くことができます。
ただし、 introspectable を他の存在しない introspectable と関連付けよう
とすると、設定のコミット時にエラーが発生します。


.. Introspectable relationships will show up in frontend system renderings of
.. introspection values.  For example, if a view registration names a route
.. name, the introspectable related to the view callable will show a reference
.. to the route to which it relates to and vice versa.

introspectable の関係は introspection 値のフロントエンドシステムの
表示結果に表れます。例えば、ビュー登録が route 名を指定すれば、
ビュー callable に関連付けられた introspectable は、それが関係する
route への参照を示し、その逆も真です。
