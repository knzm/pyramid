.. Resources

.. _resources_chapter:

リソース
=========

.. A :term:`resource` is an object that represents a "place" in a tree
.. related to your application.  Every :app:`Pyramid` application has at
.. least one resource object: the :term:`root` resource.  Even if you don't
.. define a root resource manually, a default one is created for you.  The
.. root resource is the root of a :term:`resource tree`.  A resource tree
.. is a set of nested dictionary-like objects which you can use to
.. represent your website's structure.

:term:`resource` は、アプリケーションに関係付けられたツリー上の「場所」
を表わすオブジェクトです。すべての :app:`Pyramid` アプリケーションには
少なくとも 1 つのリソースオブジェクトがあります: :term:`root` リソースです。
あなたが root リソースを手動で定義しなかったとしても、デフォルトの
root リソースが自動的に作成されます。 root リソースは :term:`resource
tree` の根です。リソースツリーは入れ子の辞書風オブジェクトの集まりで、
ウェブサイトの構造を表わすために使用することができます。


.. In an application which uses :term:`traversal` to map URLs to code, the
.. resource tree structure is used heavily to map each URL to a :term:`view
.. callable`.  When :term:`traversal` is used, :app:`Pyramid` will walk
.. through the resource tree by traversing through its nested dictionary
.. structure in order to find a :term:`context` resource.  Once a context
.. resource is found, the context resource and data in the request will be
.. used to find a :term:`view callable`.

URL をコードにマッピングするために :term:`traversal` を使用するアプリ
ケーションでは、リソースツリー構造は各 URL を :term:`view callable` に
マッピングするために頻繁に使用されます。 :term:`traversal` が使用されて
いる時、 :app:`Pyramid` は :term:`context` リソースを見つけるために、
入れ子の辞書構造をトラバースすることによってリソースツリーを渡り歩きます。
コンテキストリソースが見つかれば、 :term:`view callable` を見つける
ためにコンテキストリソースとリクエスト中のデータが使用されます。


.. In an application which uses :term:`URL dispatch`, the resource tree is only
.. used indirectly, and is often "invisible" to the developer.  In URL dispatch
.. applications, the resource "tree" is often composed of only the root resource
.. by itself.  This root resource sometimes has security declarations attached
.. to it, but is not required to have any.  In general, the resource tree is
.. much less important in applications that use URL dispatch than applications
.. that use traversal.

:term:`URL dispatch` を使用するアプリケーションでは、リソースツリーは
間接的にのみ使用され、開発者からは多くの場合「見えないもの」です。 URL
ディスパッチアプリケーションでは、リソース「ツリー」は、しばしば root
リソース自身のみから構成されます。この root リソースには、セキュリティ
宣言が取り付けられることがありますが、常にそうすることは要求されません。
一般に、トラバーサルを使用するアプリケーションに比べて URL ディスパッチを
使用するアプリケーションにおいてリソースツリーはそれほど重要ではありません。


.. In "Zope-like" :app:`Pyramid` applications, resource objects also often store
.. data persistently, and offer methods related to mutating that persistent data.
.. In these kinds of applications, resources not only represent the site
.. structure of your website, but they become the :term:`domain model` of the
.. application.

「Zope 風」の :app:`Pyramid` アプリケーションではさらに、リソースオブジェクト
はしばしばデータを永続的に格納します。そして、その永続的なデータを変化
させることに関係するメソッドを提供します。この種類のアプリケーションでは、
リソースはウェブサイトのサイト構造を表わすだけでなく、アプリケーションの
:term:`domain model` になります。


.. Also:

さらに、


.. - The ``context`` and ``containment`` predicate arguments to
..   :meth:`~pyramid.config.Configurator.add_view` (or a
..   :func:`~pyramid.view.view_config` decorator) reference a resource class
..   or resource :term:`interface`.

- :meth:`~pyramid.config.Configurator.add_view`
  (あるいは :meth:`~pyramid.view.view_config` デコレータ) に対する
  ``context`` および ``containment`` 述語引数は、
  リソースクラスまたはリソース :term:`interface` を参照します。


.. - A :term:`root factory` returns a resource.

- :term:`root factory` はリソースを返します。


.. - A resource is exposed to :term:`view` code as the :term:`context` of a
..   view.

- リソースはビューの :term:`context` として :term:`view` コードに渡されます。


.. - Various helpful :app:`Pyramid` API methods expect a resource as an argument
..   (e.g. :meth:`~pyramid.request.Request.resource_url` and others).

- 種々の有用な :app:`Pyramid` API メソッドは、引数としてリソースを期待します
  (例えば :meth:`~pyramid.request.Request.resource_url` など)。


.. index::
   single: resource tree
   single: traversal tree
   single: object tree
   single: container resources
   single: leaf resources


.. Defining a Resource Tree

リソースツリーを定義する
------------------------

.. When :term:`traversal` is used (as opposed to a purely :term:`url dispatch`
.. based application), :app:`Pyramid` expects to be able to traverse a tree
.. composed of resources (the :term:`resource tree`).  Traversal begins at a
.. root resource, and descends into the tree recursively, trying each resource's
.. ``__getitem__`` method to resolve a path segment to another resource object.
.. :app:`Pyramid` imposes the following policy on resource instances in the
.. tree:

:term:`traversal` が使用されている場合 (純粋に :term:`url dispatch` に
基づいたアプリケーションとは対照的に)、 :app:`Pyramid` はリソースから
構成されるツリー (:term:`resource tree`)をトラバースできることを期待します。
トラバースは root リソースから始まり、別のリソースオブジェクトへのパス
セグメントを解決するために各リソースの ``__getitem__`` メソッドを試しながら、
ツリーを再帰的に下って行きます。 :app:`Pyramid` はツリー上のリソース
インスタンスに以下のポリシーを課します:


.. - A container resource (a resource which contains other resources) must
..   supply a ``__getitem__`` method which is willing to resolve a unicode name
..   to a sub-resource.  If a sub-resource by a particular name does not exist
..   in a container resource, ``__getitem__`` method of the container resource
..   must raise a :exc:`KeyError`.  If a sub-resource by that name *does* exist,
..   the container's ``__getitem__`` should return the sub-resource.

- コンテナリソース (他のリソースを含むリソース) は、サブリソースへの
  unicode 名を解決するための ``__getitem__`` メソッドを提供しなければ
  なりません。特定の名前のサブリソースがコンテナリソースに存在しない場合、
  コンテナリソースの ``__getitem__`` メソッドは :exc:`KeyError` を
  投げげなければなりません。その名前のサブリソースが存在する場合、コンテナの
  ``__getitem__`` はサブリソースを返す必要があります。


.. - Leaf resources, which do not contain other resources, must not implement a
..   ``__getitem__``, or if they do, their ``__getitem__`` method must always
..   raise a :exc:`KeyError`.

- リーフリソース (他のリソースを含まないリソース) は ``__getitem__`` を
  実装してはなりません。あるいは、実装する場合 ``__getitem__`` メソッドは
  常に :exc:`KeyError` を上げなければなりません。


.. See :ref:`traversal_chapter` for more information about how traversal
.. works against resource instances.

トラバーサルがリソースインスタンスに対してどのように動作するかについての
詳細は :ref:`traversal_chapter` を参照してください。


.. Here's a sample resource tree, represented by a variable named ``root``:

これは ``root`` という名前の変数で表わされるサンプルのリソースツリーです:


.. code-block:: python
   :linenos:

    class Resource(dict):
        pass

    root = Resource({'a':Resource({'b':Resource({'c':Resource()})})})


.. The resource tree we've created above is represented by a dictionary-like
.. root object which has a single child named ``'a'``.  ``'a'`` has a single child
.. named ``'b'``, and ``'b'`` has a single child named ``'c'``, which has no
.. children. It is therefore possible to access the ``'c'`` leaf resource like so:

上で作成したリソースツリーは、 ``'a'`` という名前の子供を持つ辞書風の
root オブジェクトによって表わされます。 ``'a'`` は、 ``'b'`` という名前の
単一の子供を持っています。また、 ``'b'`` は ``'c'`` という名前の単一の子供を
持っています。 ``'c'`` は子供を持っていません。そのため、次のようにして
``'c'`` リーフリソースにアクセスすることが可能です:


.. code-block:: python
   :linenos:

   root['a']['b']['c']


.. If you returned the above ``root`` object from a :term:`root factory`, the
.. path ``/a/b/c`` would find the ``'c'`` object in the resource tree as the
.. result of :term:`traversal`.

もし :term:`root factory` から上記の ``root`` オブジェクトを返せば、
パス ``/a/b/c`` は :term:`traversal` の結果としてリソースツリー上に
``'c'`` オブジェクトを見つけるでしょう。


.. In this example, each of the resources in the tree is of the same class.
.. This is not a requirement.  Resource elements in the tree can be of any type.
.. We used a single class to represent all resources in the tree for the sake of
.. simplicity, but in a "real" app, the resources in the tree can be arbitrary.

この例において、ツリーの各々のリソースは同じクラスです。これは必須では
ありません。ツリーのリソース要素は任意の型になることができます。私たちは、
単純性のためにツリーのすべてのリソースを表わすために単一のクラスを使用
しました。しかし、「実際の」アプリでは、ツリーのリソースは任意のクラスが
可能です。


.. Although the example tree above can service a traversal, the resource
.. instances in the above example are not aware of :term:`location`, so their
.. utility in a "real" application is limited.  To make best use of built-in
.. :app:`Pyramid` API facilities, your resources should be "location-aware".
.. The next section details how to make resources location-aware.

上記のサンプルのツリーはトラバーサルを実行することができますが、上記の
例におけるリソースインスタンスは :term:`location` aware ではありません。
したがって、「実際の」アプリケーションでの有用性は制限されています。
内蔵の :app:`Pyramid` API 機能を最良に利用するために、リソースは
「location-aware (位置を意識)」すべきです。次のセクションは、リソースを
location-aware にする方法を詳しく説明します。


.. index::
   pair: location-aware; resource


.. Location-Aware Resources

.. _location_aware:

location aware なリソース
-------------------------

.. In order for certain :app:`Pyramid` location, security, URL-generation, and
.. traversal APIs to work properly against the resources in a resource tree, all
.. resources in the tree must be :term:`location` -aware.  This means they must
.. have two attributes: ``__parent__`` and ``__name__``.

適切な :app:`Pyramid` ロケーション、セキュリティ、 URL 生成および
トラバーサル API がリソースツリー上のリソースに対して適切に働くために、
ツリー上のすべてのリソースは :term:`location` -aware である必要があります。
これは、それらのリソースが ``__parent__`` と ``__name__`` という 2 つの
属性を持っている必要があるということを意味します。


.. The ``__parent__`` attribute of a location-aware resource should be a
.. reference to the resource's parent resource instance in the tree.  The
.. ``__name__`` attribute should be the name with which a resource's parent
.. refers to the resource via ``__getitem__``.

location-aware なリソースの ``__parent__`` 属性は、そのリソースのツリー
上の親リソースインスタンスへの参照でなければなりません。 ``__name__``
属性はリソースの親が ``__getitem__`` によってそのリソースを参照する時の
名前でなければなりません。


.. The ``__parent__`` of the root resource should be ``None`` and its
.. ``__name__`` should be the empty string.  For instance:

root リソースの ``__parent__`` は ``None`` でなければなりません。また、
その ``__name__`` は空文字列でなければなりません。例えば:


.. code-block:: python
   :linenos:

   class MyRootResource(object):
       __name__ = ''
       __parent__ = None


.. A resource returned from the root resource's ``__getitem__`` method should
.. have a ``__parent__`` attribute that is a reference to the root resource, and
.. its ``__name__`` attribute should match the name by which it is reachable via
.. the root resource's ``__getitem__``.  A container resource within the root
.. resource should have a ``__getitem__`` that returns resources with a
.. ``__parent__`` attribute that points at the container, and these subobjects
.. should have a ``__name__`` attribute that matches the name by which they are
.. retrieved from the container via ``__getitem__``.  This pattern continues
.. recursively "up" the tree from the root.

root リソースの ``__getitem__`` メソッドから返されたリソースは、 root
リソースへの参照である ``__parent__`` 属性を持っていなければなりません。
また、その ``__name__`` 属性は、 root リソースの ``__getitem__`` によって
到達可能な名前と一致しなければなりません。 root リソース内のコンテナリ
ソースは、コンテナを指す ``__parent__`` 属性を備えたリソースを返す
``__getitem__`` を持たなければなりません。また、これらのサブオブジェクトは、
``__getitem__`` によってコンテナから検索される名前と一致する ``__name__``
属性を持たなければなりません。このパターンは、 root から木を「上に」
再帰的に継続します。


.. The ``__parent__`` attributes of each resource form a linked list that points
.. "downwards" toward the root. This is analogous to the `..` entry in
.. filesystem directories. If you follow the ``__parent__`` values from any
.. resource in the resource tree, you will eventually come to the root resource,
.. just like if you keep executing the ``cd ..`` filesystem command, eventually
.. you will reach the filesystem root directory.

各リソースの ``__parent__`` 属性は、 root に向かって「下方へ」指すリンク
リストを形成します。これはファイルシステムのディレクトリ中の `..` エントリ
と類似しています。リソースツリーの任意のリソースから ``__parent__`` 値
を追って行けば、やがて root リソースに達するでしょう。ちょうど、ファイル
システムコマンド ``cd ..`` を実行し続けるといずれファイルシステムのルート
ディレクトリーに達するように。


.. warning::

   .. If your root resource has a ``__name__`` argument that is not
   .. ``None`` or the empty string, URLs returned by the
   .. :func:`~pyramid.request.Request.resource_url` function and paths generated
   .. by the :func:`~pyramid.traversal.resource_path` and
   .. :func:`~pyramid.traversal.resource_path_tuple` APIs will be generated
   .. improperly.  The value of ``__name__`` will be prepended to every path and
   .. URL generated (as opposed to a single leading slash or empty tuple
   .. element).

   root リソースが ``None`` または空文字列でない ``__name__`` 引数を
   持っていると、 :func:`~pyramid.request.Request.resource_url` 関数に
   よって返された URLおよび :func:`~pyramid.traversal.resource_path` と
   :func:`~pyramid.traversal.resource_path_tuple` API によって生成された
   パスは、不適当に生成されるでしょう。 ``__name__`` の値は、生成された
   すべてのパスおよび URL に前置されます (単一の先頭のスラッシュあるいは
   空のタプル要素とは対照的に)。


  .. Using :mod:`pyramid_traversalwrapper`

.. sidebar:: \ :mod:`pyramid_traversalwrapper` を使う


  .. If you'd rather not manage the ``__name__`` and ``__parent__`` attributes
  .. of your resources "by hand", an add-on package named
  .. :mod:`pyramid_traversalwrapper` can help.

  リソースの ``__name__`` と ``__parent__`` 属性を「手動で」管理したく
  なければ、 :mod:`pyramid_traversalwrapper` という名前のアドオン
  パッケージは助けになりえます。


  .. In order to use this helper feature, you must first install the
  .. :mod:`pyramid_traversalwrapper` package (available via PyPI), then register
  .. its ``ModelGraphTraverser`` as the traversal policy, rather than the
  .. default :app:`Pyramid` traverser. The package contains instructions for
  .. doing so.

  このヘルパー機能を使用するためには、最初に
  :mod:`pyramid_traversalwrapper` パッケージをインストールして (PyPI
  から入手可能)、次にデフォルトの :app:`Pyramid` トラバーサーではなく、
  トラバーサルポリシーとしてその ``ModelGraphTraverser`` を登録する必要
  があります。それをするための指示がパッケージに含まれています。


  .. Once :app:`Pyramid` is configured with this feature, you will no longer
  .. need to manage the ``__parent__`` and ``__name__`` attributes on resource
  .. objects "by hand".  Instead, as necessary, during traversal :app:`Pyramid`
  .. will wrap each resource (even the root resource) in a ``LocationProxy``
  .. which will dynamically assign a ``__name__`` and a ``__parent__`` to the
  .. traversed resource (based on the last traversed resource and the name
  .. supplied to ``__getitem__``).  The root resource will have a ``__name__``
  .. attribute of ``None`` and a ``__parent__`` attribute of ``None``.

  一旦 :app:`Pyramid` にこの機能が設定されれば、もはや「手動で」リソース
  オブジェクト上の ``__parent__`` と ``__name__`` 属性を管理する必要は
  ありません。代わりに、必要に応じて、トラバーサル中に :app:`Pyramid` は
  トラバースされたリソースに (最後にトラバーサルされたリソースおよび
  ``__getitem__`` に提供された  名前に基づいて)ダイナミックに
  ``__name__`` と ``__parent__`` を設定する ``LocationProxy`` で各リソース
  (root リソースさえ) をラップします。 root リソースは、 ``None`` の
  ``__name__`` 属性および ``None`` の ``__parent__`` 属性を持ちます。


.. Applications which use tree-walking :app:`Pyramid` APIs require
.. location-aware resources.  These APIs include (but are not limited to)
.. :meth:`~pyramid.request.Request.resource_url`,
.. :func:`~pyramid.traversal.find_resource`,
.. :func:`~pyramid.traversal.find_root`,
.. :func:`~pyramid.traversal.find_interface`,
.. :func:`~pyramid.traversal.resource_path`,
.. :func:`~pyramid.traversal.resource_path_tuple`, or
.. :func:`~pyramid.traversal.traverse`, :func:`~pyramid.traversal.virtual_root`,
.. and (usually) :func:`~pyramid.security.has_permission` and
.. :func:`~pyramid.security.principals_allowed_by_permission`.

ツリーをたどるための :app:`Pyramid` API を使用するアプリケーションは
location-aware なリソースを必要とします。これらの API は、以下のもの
を含んでいます (しかしこれだけに限りません):
:meth:`~pyramid.request.Request.resource_url`,
:func:`~pyramid.traversal.find_resource`,
:func:`~pyramid.traversal.find_root`,
:func:`~pyramid.traversal.find_interface`,
:func:`~pyramid.traversal.resource_path`,
:func:`~pyramid.traversal.resource_path_tuple`, あるいは
:func:`~pyramid.traversal.traverse`, :func:`~pyramid.traversal.virtual_root`,
そして (通常は) :func:`~pyramid.security.has_permission` と
:func:`~pyramid.security.principals_allowed_by_permission`.


.. In general, since so much :app:`Pyramid` infrastructure depends on
.. location-aware resources, it's a good idea to make each resource in your tree
.. location-aware.

一般に、非常に多くの :app:`Pyramid` 基盤が location-aware なリソースに
依存しているので、ツリーに含まれる各リソースを location-aware にすることは
良い考えです。


.. index::
   single: resource_url
   pair: generating; resource url


.. Generating The URL Of A Resource

.. _generating_the_url_of_a_resource:

リソースの URL を生成する
--------------------------------

.. If your resources are :term:`location` aware, you can use the
.. :meth:`pyramid.request.Request.resource_url` API to generate a URL for the
.. resource.  This URL will use the resource's position in the parent tree to
.. create a resource path, and it will prefix the path with the current
.. application URL to form a fully-qualified URL with the scheme, host, port,
.. and path.  You can also pass extra arguments to
.. :meth:`~pyramid.request.Request.resource_url` to influence the generated URL.

リソースが :term:`location` aware である場合、リソース用の URL を生成す
るために :meth:`pyramid.request.Request.resource_url` API を使用することが
できます。この URL は、リソースパスを生成するためにリソースの親ツリー
における位置を使用します。また、それは、スキーム、ホスト、ポートおよび
パスを持つ完全修飾 URLを形成するためにパスの前に現在のアプリケーション
URL を付けます。さらに、生成された URL に影響を及ぼすために追加の引数を
:meth:`~pyramid.request.Request.resource_url` へ渡すことができます。


.. The simplest call to :meth:`~pyramid.request.Request.resource_url` looks like
.. this:

:meth:`~pyramid.request.Request.resource_url` への最も単純な呼び出しは
このようになります:


.. code-block:: python
   :linenos:

   url = request.resource_url(resource)


.. The ``request`` in the above example is an instance of a :app:`Pyramid`
.. :term:`request` object.

上記の例で ``request`` は :app:`Pyramid` :term:`request` オブジェクトの
インスタンスです。


.. If the resource referred to as ``resource`` in the above example was the root
.. resource, and the host that was used to contact the server was
.. ``example.com``, the URL generated would be ``http://example.com/``.
.. However, if the resource was a child of the root resource named ``a``, the
.. generated URL would be ``http://example.com/a/``.

上記の例で ``resource`` として参照されるリソースが root リソースで、
サーバにコンタクトするために使用されたホストが ``example.com`` だった場合、
生成される URL は ``http://example.com/`` になります。しかし、もしリソースが
``a`` という名前の root リソースの子供なら、生成される URL は
``http://example.com/a/`` になります。


.. A slash is appended to all resource URLs when
.. :meth:`~pyramid.request.Request.resource_url` is used to generate them in
.. this simple manner, because resources are "places" in the hierarchy, and URLs
.. are meant to be clicked on to be visited.  Relative URLs that you include on
.. HTML pages rendered as the result of the default view of a resource are more
.. apt to be relative to these resources than relative to their parent.

:meth:`pyramid.request.Request.resource_url` を使用してこの単純な方法で
リソース URL を生成する場合、すべてのリソース URL にはスラッシュが追加
されます。なぜなら、リソースは階層における「場所」であり、 URL はそこを
訪れるためにクリックされるべきものだからです。リソースのデフォルトビュー
の結果としてレンダリングされた HTML ページに含まれる相対 URL は、
その親に対してではなく、そのリソースに対して相対的であることが適切です。


.. You can also pass extra elements to
.. :meth:`~pyramid.request.Request.resource_url`:

さらに、追加の要素を :meth:`~pyramid.request.Request.resource_url` へ
渡すことができます:


.. code-block:: python
   :linenos:

   url = request.resource_url(resource, 'foo', 'bar')


.. If the resource referred to as ``resource`` in the above example was the root
.. resource, and the host that was used to contact the server was
.. ``example.com``, the URL generated would be ``http://example.com/foo/bar``.
.. Any number of extra elements can be passed to
.. :meth:`~pyramid.request.Request.resource_url` as extra positional arguments.
.. When extra elements are passed, they are appended to the resource's URL.  A
.. slash is not appended to the final segment when elements are passed.

上記の例で ``resource`` として参照されるリソースが root リソースで、
サーバにコンタクトするために使用されたホストが ``example.com`` だった場合、
生成される URL は ``http://example.com/foo/bar`` になります。任意の数の追加
の要素を追加の位置引数として :meth:`~pyramid.request.Request.resource_url`
に渡すことができます。追加の要素が渡された場合、それらはリソースの URL
に追加されます。要素が渡された場合、最終セグメントにスラッシュは追加さ
れません。


.. You can also pass a query string:

さらに、クエリ文字列を渡すこともできます:


.. code-block:: python
   :linenos:

   url = request.resource_url(resource, query={'a':'1'})


.. If the resource referred to as ``resource`` in the above example was the root
.. resource, and the host that was used to contact the server was
.. ``example.com``, the URL generated would be ``http://example.com/?a=1``.

上記の例で ``resource`` として参照されるリソースが root リソースで、
サーバにコンタクトするために使用されたホストが ``example.com`` だった場合、
生成される URL は ``http://example.com/?a=1`` になります。


.. When a :term:`virtual root` is active, the URL generated by
.. :meth:`~pyramid.request.Request.resource_url` for a resource may be "shorter"
.. than its physical tree path.  See :ref:`virtual_root_support` for more
.. information about virtually rooting a resource.

:term:`virtual root` が有効な場合、
:meth:`~pyramid.request.Request.resource_url` によってリソースのために
生成された URL は、その物理的なツリーパスより「短い」ことがあります。
リソースの仮想的な root 変更についての詳細は
:ref:`virtual_root_support` を参照してください。


.. For more information about generating resource URLs, see the documentation
.. for :meth:`pyramid.request.Request.resource_url`.

リソース URL の生成に関する詳細は
:meth:`pyramid.request.Request.resource_url` のドキュメンテーションを
参照してください。


.. index::
   pair: resource URL generation; overriding


.. Overriding Resource URL Generation

.. _overriding_resource_url_generation:

リソース URL 生成のオーバーライド
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. If a resource object implements a ``__resource_url__`` method, this method
.. will be called when :meth:`~pyramid.request.Request.resource_url` is called
.. to generate a URL for the resource, overriding the default URL returned for
.. the resource by :meth:`~pyramid.request.Request.resource_url`.

リソースオブジェクトが ``__resource_url__`` メソッドを実装していれば、
このリソース用に URL を生成するために
:meth:`~pyramid.request.Request.resource_url` が呼ばれる時、
:meth:`~pyramid.request.Request.resource_url` によって返されるデフォルト
URL を上書きしてこのメソッドが呼ばれます。


.. The ``__resource_url__`` hook is passed two arguments: ``request`` and
.. ``info``.  ``request`` is the :term:`request` object passed to
.. :meth:`~pyramid.request.Request.resource_url`.  ``info`` is a dictionary with
.. two keys:

``__resource_url__`` フックには ``request`` と ``info`` という 2 つの引数
が渡されます。 ``request`` は :meth:`~pyramid.request.Request.resource_url`
に渡された ``request`` オブジェクトです。 ``info`` は 2 つのキーを持つ辞書です
(訳注: 「3 つ」の間違い？):


``physical_path``

   .. A string representing the "physical path" computed for the resource, as
   .. defined by ``pyramid.traversal.resource_path(resource)``.  It will begin
   .. and end with a slash.

   そのリソースのために計算された「物理的パス」を表わす文字列。
   ``pyramid.traversal.resource_path(resource)`` によって定義されます。
   スラッシュで始まりスラッシュで終わります。


``virtual_path``

   .. A string representing the "virtual path" computed for the resource, as
   .. defined by :ref:`virtual_root_support`.  This will be identical to the
   .. physical path if virtual rooting is not enabled.  It will begin and end
   .. with a slash.

   そのリソースのために計算された「仮想的パス」を表わす文字列。
   :ref:`virtual_root_support` によって定義されます。
   仮想 root 変更が有効でなければ、これは物理的パスと同一でしょう。
   スラッシュで始まりスラッシュで終わります。


``app_url``

  .. A string representing the application URL generated during
  .. ``request.resource_url``.  It will not end with a slash.  It represents a
  .. potentially customized URL prefix, containing potentially custom scheme,
  .. host and port information passed by the user to ``request.resource_url``.
  .. It should be preferred over use of ``request.application_url``.

  ``request.resource_url`` の中で生成されたアプリケーション URL を
  表わす文字列。スラッシュで終わりません。これは、潜在的にカスタマイズ
  された URL 接頭辞を表し、ユーザによって ``request.resource_url`` に
  渡されたカスタムなスキーム、ホストおよびポート情報を潜在的に含みます。
  ``request.application_url`` よりもこちらを使用することが推奨されます。


.. The ``__resource_url__`` method of a resource should return a string
.. representing a URL.  If it cannot override the default, it should return
.. ``None``.  If it returns ``None``, the default URL will be returned.

リソースの ``__resource_url__`` メソッドは、 URL を表わす文字列を返す
必要があります。デフォルトを上書きできない場合 ``None`` を返すべきです。
このメソッドが ``None`` を返せば、デフォルト URL が返されるでしょう。


.. Here's an example ``__resource_url__`` method.

これはサンプルの ``__resource_url__`` メソッドです。


.. code-block:: python
   :linenos:

   class Resource(object):
       def __resource_url__(self, request, info):
           return info['app_url'] + info['virtual_path']


.. The above example actually just generates and returns the default URL, which
.. would have been what was generated by the default ``resource_url`` machinery,
.. but your code can perform arbitrary logic as necessary.  For example, your
.. code may wish to override the hostname or port number of the generated URL.

上記の例は、実際には単にデフォルトの URL を生成して返します。それは
デフォルトの ``resource_url`` 機構によって生成されたはずのものです。
しかし、このコードは必要に応じて任意のロジックを実行できます。例えば、
あなたのコードでは、生成された URL のホスト名またはポート番号を無視
したいと思うかもしれません。


.. Note that the URL generated by ``__resource_url__`` should be fully
.. qualified, should end in a slash, and should not contain any query string or
.. anchor elements (only path elements) to work with
.. :meth:`~pyramid.request.Request.resource_url`.

:meth:`~pyramid.request.Request.resource_url` とともに働くために、
``__resource_url__`` によって生成される URL は、完全修飾形式で、
スラッシュで終わり、クエリ文字列あるいはアンカー要素を含むべきでない
(パス要素だけ)ということに注意してください。


.. index::
   single: resource path generation


.. Generating the Path To a Resource

リソースへのパスの生成
---------------------------------

.. :func:`pyramid.traversal.resource_path` returns a string object representing
.. the absolute physical path of the resource object based on its position in
.. the resource tree.  Each segment of the path is separated with a slash
.. character.

:func:`pyramid.traversal.resource_path` は、リソースツリー上の位置
に基づいてリソースオブジェクトの絶対的な物理的パスを表わす文字列オブジェクト
を返します。パスのセグメントはそれぞれスラッシュ文字で分離されています。


.. code-block:: python
   :linenos:

   from pyramid.traversal import resource_path
   url = resource_path(resource)


.. If ``resource`` in the example above was accessible in the tree as
.. ``root['a']['b']``, the above example would generate the string ``/a/b``.

もし上記の例で ``resource`` がツリー上で ``root['a']['b']`` として
アクセス可能ならば、上記の例は文字列 ``/a/b`` を生成するでしょう。


.. Any positional arguments passed in to :func:`~pyramid.traversal.resource_path`
.. will be appended as path segments to the end of the resource path.

:func:`~pyramid.traversal.resource_path` に渡されたすべての位置引数も
リソースパスの末端にパスセグメントとして追加されます。


.. code-block:: python
   :linenos:

   from pyramid.traversal import resource_path
   url = resource_path(resource, 'foo', 'bar')


.. If ``resource`` in the example above was accessible in the tree as
.. ``root['a']['b']``, the above example would generate the string
.. ``/a/b/foo/bar``.

もし上記の例で ``resource`` がツリー上で ``root['a']['b']`` として
アクセス可能なら、上記の例は文字列 ``/a/b/foo/bar`` を生成するでしょう。


.. The resource passed in must be :term:`location`-aware.

渡されたリソースは `location` aware でなければなりません。


.. The presence or absence of a :term:`virtual root` has no impact on the
.. behavior of :func:`~pyramid.traversal.resource_path`.

:term:`virtual root` が存在するかどうかは
:func:`~*pyramid.traversal.resource_path` の振る舞いに影響を及ぼしません。


.. index::
   pair: resource; finding by path


.. Finding a Resource by Path

パスからリソースを見つける
--------------------------

.. If you have a string path to a resource, you can grab the resource from
.. that place in the application's resource tree using
.. :func:`pyramid.traversal.find_resource`.

リソースへの文字列パスを持っていれば、
:func:`pyramid.traversal.find_resource` を使ってアプリケーションの
リソースツリー場所からリソースを取得することができます。


.. You can resolve an absolute path by passing a string prefixed with a ``/`` as
.. the ``path`` argument:

``/`` プリフィックスを持った文字列を ``path`` 引数として渡すことによって、
絶対パスを解決することができます:


.. code-block:: python
   :linenos:

   from pyramid.traversal import find_resource
   url = find_resource(anyresource, '/path')


.. Or you can resolve a path relative to the resource you pass in by passing a
.. string that isn't prefixed by ``/``:

あるいは、 ``/`` プリフィックスを持たない文字列を渡すことにより、
指定したリソースからの相対パスを解決することができます:


.. code-block:: python
   :linenos:

   from pyramid.traversal import find_resource
   url = find_resource(anyresource, 'path')


.. Often the paths you pass to :func:`~pyramid.traversal.find_resource` are
.. generated by the :func:`~pyramid.traversal.resource_path` API.  These APIs
.. are "mirrors" of each other.

:func:`~pyramid.traversal.find_resource` に渡すパスはしばしば
:func:`~pyramid.traversal.resource_path` API によって生成されます。
これらの API は互いの「鏡」です。


.. If the path cannot be resolved when calling
.. :func:`~pyramid.traversal.find_resource` (if the respective resource in the
.. tree does not exist), a :exc:`KeyError` will be raised.

:func:`~pyramid.traversal.find_resource` を呼び出したときにパスを解決
できなければ (ツリー上のそれぞれのリソースが存在しなければ) 、
:exc:`KeyError` 例外が投げられます。


.. See the :func:`pyramid.traversal.find_resource` documentation for more
.. information about resolving a path to a resource.

パスからリソースへの解決に関する詳細は
:func:`pyramid.traversal.find_resource` ドキュメンテーションを参照して
ください。


.. index::
   pair: resource; lineage


.. Obtaining the Lineage of a Resource

リソースの lineage の取得
-----------------------------------

.. :func:`pyramid.location.lineage` returns a generator representing the
.. :term:`lineage` of the :term:`location` aware :term:`resource` object.

:func:`pyramid.location.lineage` は、 :term:`location` aware な
:term:`resource` オブジェクトの :term:`lineage` (系統, 血統) を表わす
ジェネレータを返します。


.. The :func:`~pyramid.location.lineage` function returns the resource it is
.. passed, then each parent of the resource, in order.  For example, if the
.. resource tree is composed like so:

:func:`pyramid.location.lineage` 関数は渡されたリソースを返し、その後
順番にリソースの親をそれぞれ返します。例えば、リソースツリーが以下の
ように構成される場合:


.. code-block:: python
   :linenos:

   class Thing(object): pass

   thing1 = Thing()
   thing2 = Thing()
   thing2.__parent__ = thing1


.. Calling ``lineage(thing2)`` will return a generator.  When we turn it into a
.. list, we will get:

``lineage(thing2)`` の呼び出しはジェネレータを返します。それをリストに
変換すると、次のような結果を得るでしょう:


.. code-block:: python
   :linenos:

   list(lineage(thing2))
   [ <Thing object at thing2>, <Thing object at thing1> ]


.. The generator returned by :func:`~pyramid.location.lineage` first returns the
.. resource it was passed unconditionally.  Then, if the resource supplied a
.. ``__parent__`` attribute, it returns the resource represented by
.. ``resource.__parent__``.  If *that* resource has a ``__parent__`` attribute,
.. return that resource's parent, and so on, until the resource being inspected
.. either has no ``__parent__`` attribute or has a ``__parent__`` attribute of
.. ``None``.

:func:`~pyramid.location.lineage` によって返されたジェネレータは、最初に
渡されたリソースを無条件に返します。次に、そのリソースが
``__parent__`` 属性を持っている場合、 ``resource.__parent__`` によって
表わされるリソースを返します。もし *その* リソースが ``__parent__`` 属性
を持っている場合、そのリソースの親を返します。検査されているリソースが
``__parent__`` 属性を持たないか、 ``__parent__`` 属性が ``None`` になる
までこれが続きます。


.. See the documentation for :func:`pyramid.location.lineage` for more
.. information.

詳細は、 :func:`pyramid.location.lineage` のドキュメンテーションを参照
してください。


.. Determining if a Resource is In The Lineage of Another Resource

リソースが別のリソースの lineage かどうかの判断
---------------------------------------------------------------

.. Use the :func:`pyramid.location.inside` function to determine if one resource
.. is in the :term:`lineage` of another resource.

あるリソースが別のリソースの :term:`lineage` であるかどうかを判断するには、
:func:`pyramid.location.inside` 関数を使用してください。


.. For example, if the resource tree is:

例えば、リソースツリーがこのような場合:


.. code-block:: python
   :linenos:

   class Thing(object): pass

   a = Thing()
   b = Thing()
   b.__parent__ = a


.. Calling ``inside(b, a)`` will return ``True``, because ``b`` has a lineage
.. that includes ``a``.  However, calling ``inside(a, b)`` will return ``False``
.. because ``a`` does not have a lineage that includes ``b``.

``b`` は ``a`` を含む lineage を持つので、 ``inside(b, a)`` の呼び出しは
``True`` を返すでしょう。しかし、 ``a`` は ``b`` を含む lineage を持たない
ので、 ``inside(a, b)`` の呼び出しは ``False`` を返すでしょう。


.. The argument list for :func:`~pyramid.location.inside` is ``(resource1,
.. resource2)``.  ``resource1`` is 'inside' ``resource2`` if ``resource2`` is a
.. :term:`lineage` ancestor of ``resource1``.  It is a lineage ancestor if its
.. parent (or one of its parent's parents, etc.) is an ancestor.

:func:`~pyramid.location.inside` の引数リストは ``(resource1,
resource2)`` です。 ``resource2`` が ``resource1`` の :term:`lineage`
祖先である場合、 ``resource1`` は ``resource2`` の inside です。その親
(あるいはその親の親などのうちの1つ) が祖先ならば、それは lineage 祖先です。


.. See :func:`pyramid.location.inside` for more information.

詳細は、 :func:`pyramid.location.inside` を参照してください。


.. index::
   pair: resource; finding root


.. Finding the Root Resource

root リソースを見つける
-------------------------

.. Use the :func:`pyramid.traversal.find_root` API to find the :term:`root`
.. resource.  The root resource is the root resource of the :term:`resource
.. tree`.  The API accepts a single argument: ``resource``.  This is a resource
.. that is :term:`location` aware.  It can be any resource in the tree for which
.. you want to find the root.

:term:`root` リソースを見つけるためには
:func:`pyramid.traversal.find_root` API を使用してください。この root
リソースは :term:`resource tree` の root リソースです。この API は単一
の引数 ``resource`` を受け取ります。 ``resource`` は ``location`` aware
なリソースです。ツリー上で root を見つけたいと思う任意のリソースを渡す
ことができます。


.. For example, if the resource tree is:

例えば、リソースツリーが次のような場合:


.. code-block:: python
   :linenos:

   class Thing(object): pass

   a = Thing()
   b = Thing()
   b.__parent__ = a


.. Calling ``find_root(b)`` will return ``a``.

``find_root(b)`` の呼び出しは ``a`` を返すでしょう。


.. The root resource is also available as ``request.root`` within :term:`view
.. callable` code.

root リソースは :term:`view callable` コード内では ``request.root`` として
もアクセス可能です。


.. The presence or absence of a :term:`virtual root` has no impact on the
.. behavior of :func:`~pyramid.traversal.find_root`.  The root object returned
.. is always the *physical* root object.

:term:`virtual root` の有無は :func:`pyramid.traversal.find_root` の
振る舞いに影響を及ぼしません。返された root オブジェクトは常に
*物理的な* root オブジェクトです。


.. index::
   single: resource interfaces


.. Resources Which Implement Interfaces

.. _resources_which_implement_interfaces:

インタフェースを実装するリソース
------------------------------------

.. Resources can optionally be made to implement an :term:`interface`.  An
.. interface is used to tag a resource object with a "type" that can later be
.. referred to within :term:`view configuration` and by
.. :func:`pyramid.traversal.find_interface`.

リソースは任意で :term:`interface` を実装するように作ることができます。
インタフェースはリソースオブジェクトに「型」を用いてタグ付けするために
使用されます。型は、その後 :term:`view configuration` の内で参照することが
でき、 :func:`pyramid.traversal.find_interface` によって参照されることがで
きます。


.. Specifying an interface instead of a class as the ``context`` or
.. ``containment`` predicate arguments within :term:`view configuration`
.. statements makes it possible to use a single view callable for more than one
.. class of resource object.  If your application is simple enough that you see
.. no reason to want to do this, you can skip reading this section of the
.. chapter.

:term:`view configuration` ステートメント内の ``context`` または
``containment`` 述語引数としてクラスの代わりにインタフェースを指定する
ことで、単一のビュー callable をリソースオブジェクトの複数のクラスに対して
使用することができます。これをしたい理由が分からないくらいにあなたの
アプリケーションが単純な場合、このセクションを読むのをスキップできます。


.. For example, here's some code which describes a blog entry which also
.. declares that the blog entry implements an :term:`interface`.

例えば、これはブログエントリについて記述するコードで、ブログエントリが
:term:`interface` を実装すると宣言しています。


.. code-block:: python
   :linenos:

   import datetime
   from zope.interface import implementer
   from zope.interface import Interface

   class IBlogEntry(Interface):
       pass

   @implementer(IBlogEntry)
   class BlogEntry(object):
       def __init__(self, title, body, author):
           self.title = title
           self.body = body
           self.author = author
           self.created = datetime.datetime.now()


.. This resource consists of two things: the class which defines the resource
.. constructor as the class ``BlogEntry``, and an :term:`interface` attached to
.. the class via an ``implementer`` class decorator using the ``IBlogEntry``
.. interface as its sole argument.

このリソースは 2 つのものから構成されます: ``BlogEntry`` クラスという
リソースコンストラクタを定義するクラスと、唯一の引数として
``IBlogEntry`` インタフェースを用いてクラスデコレータ ``implementer``
によってクラスに取り付けられた :term:`interface` です。


.. The interface object used must be an instance of a class that inherits from
.. :class:`zope.interface.Interface`.

使用されるインタフェースオブジェクトは :class:`zope.interface.Interface`
から継承するクラスのインスタンスでなければなりません。


.. A resource class may implement zero or more interfaces.  You specify that a
.. resource implements an interface by using the
.. :func:`zope.interface.implementer` function as a class decorator.  The above
.. ``BlogEntry`` resource implements the ``IBlogEntry`` interface.

リソースクラスは 0 個以上のインタフェースを実装することができます。
クラスデコレータとして :func:`zope.interface.implementer` 関数を使用する
ことにより、リソースがインタフェースを実装することを明示します。
上記の ``BlogEntry`` リソースは ``IBlogEntry`` インタフェースを実装
しています。


.. You can also specify that a particular resource *instance* provides an
.. interface, as opposed to its class.  When you declare that a class implements
.. an interface, all instances of that class will also provide that interface.
.. However, you can also just say that a single object provides the interface.
.. To do so, use the :func:`zope.interface.directlyProvides` function:

さらに、クラスではなく特定のリソース *インスタンス* がインタフェースを
提供することを明示することもできます。クラスがインタフェースを実装する
と宣言した場合、そのクラスのすべてのインスタンスもそのインタフェースを
提供するようになります。しかし、単一のオブジェクトがインタフェースを
提供すると単に言うこともできます。そのためには
:func:`zope.interface.directlyProvides` 関数を使用してください:


.. code-block:: python
   :linenos:

   import datetime
   from zope.interface import directlyProvides
   from zope.interface import Interface

   class IBlogEntry(Interface):
       pass

   class BlogEntry(object):
       def __init__(self, title, body, author):
           self.title = title
           self.body = body
           self.author = author
           self.created = datetime.datetime.now()

   entry = BlogEntry('title', 'body', 'author')
   directlyProvides(entry, IBlogEntry)


.. :func:`zope.interface.directlyProvides` will replace any existing interface
.. that was previously provided by an instance.  If a resource object already
.. has instance-level interface declarations that you don't want to replace, use
.. the :func:`zope.interface.alsoProvides` function:

:func:`zope.interface.directlyProvides` は、それ以前にインスタンスによって
提供されていた既存のあらゆるインタフェースを置き換えます。リソースオブジェクト
が既にインスタンスレベルのインタフェース宣言をしていて置き換えたくない
場合は、 :func:`zope.interface.alsoProvides` 関数を使用してください:


.. code-block:: python
   :linenos:

   import datetime
   from zope.interface import alsoProvides
   from zope.interface import directlyProvides
   from zope.interface import Interface

   class IBlogEntry1(Interface):
       pass

   class IBlogEntry2(Interface):
       pass

   class BlogEntry(object):
       def __init__(self, title, body, author):
           self.title = title
           self.body = body
           self.author = author
           self.created = datetime.datetime.now()

   entry = BlogEntry('title', 'body', 'author')
   directlyProvides(entry, IBlogEntry1)
   alsoProvides(entry, IBlogEntry2)


.. :func:`zope.interface.alsoProvides` will augment the set of interfaces
.. directly provided by an instance instead of overwriting them like
.. :func:`zope.interface.directlyProvides` does.

:func:`zope.interface.alsoProvides` は、
:func:`zope.interface.directlyProvides` のようにインスタンスによって
直接提供されるインタフェースの集合を上書きする代わりに、追加します。


.. For more information about how resource interfaces can be used by view
.. configuration, see :ref:`using_resource_interfaces`.

ビュー設定でリソースインタフェースがどのように使用されるかについての
詳細は :ref:`using_resource_interfaces` を参照してください。


.. index::
   pair: resource; finding by interface or class


.. Finding a Resource With a Class or Interface in Lineage

クラスまたはインタフェースを使って lineage からリソースを見つける
-----------------------------------------------------------------

.. Use the :func:`~pyramid.traversal.find_interface` API to locate a parent that
.. is of a particular Python class, or which implements some :term:`interface`.

特定の Python クラスの、あるいは特定の :term:`interface` を実装する
親を見つけるには :func:`~pyramid.traversal.find_interface` API を
使用してください。


.. For example, if your resource tree is composed as follows:

例えば、リソースツリーが以下のように構成される場合:


.. code-block:: python
   :linenos:

   class Thing1(object): pass
   class Thing2(object): pass

   a = Thing1()
   b = Thing2()
   b.__parent__ = a


.. Calling ``find_interface(a, Thing1)`` will return the ``a`` resource because
.. ``a`` is of class ``Thing1`` (the resource passed as the first argument is
.. considered first, and is returned if the class or interface spec matches).

``a`` がクラス ``Thing1`` なので、 ``find_interface(a, Thing1)`` の呼び
出しは ``a`` リソースを返すでしょう (最初の引数として渡されたリソースが
最初に考慮され、クラスまたはインタフェースのスペックが一致する場合それが
返されます)。


.. Calling ``find_interface(b, Thing1)`` will return the ``a`` resource because
.. ``a`` is of class ``Thing1`` and ``a`` is the first resource in ``b``'s
.. lineage of this class.

``a`` がクラス ``Thing1`` で、 ``b`` の lineage の中で ``a`` がこのクラス
の最初のリソースなので、 ``find_interface(b, Thing1)`` の呼び出しは
``a`` リソースを返すでしょう。


.. Calling ``find_interface(b, Thing2)`` will return the ``b`` resource.

``find_interface(b, Thing2)`` の呼び出しは ``b`` リソースを返すでしょう。


.. The second argument to find_interface may also be a :term:`interface` instead
.. of a class.  If it is an interface, each resource in the lineage is checked
.. to see if the resource implements the specificed interface (instead of seeing
.. if the resource is of a class).  See also
.. :ref:`resources_which_implement_interfaces`.

find_interface への第 2 引数は、クラスの代わりに :term:`interface` を
使うことも可能です。これがインタフェースの場合、 lineage 中のそれぞれ
のリソースが指定されたインタフェースを実装するかどうかチェックされます
(リソースがあるクラスかどうか確かめる代わりに)。
:ref:`resources_which_implement_interfaces` も参照してください。


.. index::
   single: resource API functions
   single: url generation (traversal)


.. :app:`Pyramid` API Functions That Act Against Resources

リソースに対して動作する :app:`Pyramid` API 関数群
-------------------------------------------------------

.. A resource object is used as the :term:`context` provided to a view.  See
.. :ref:`traversal_chapter` and :ref:`urldispatch_chapter` for more information
.. about how a resource object becomes the context.

リソースオブジェクトはビューに提供される :term:`context` として使用され
ます。リソースオブジェクトがどのようにしてコンテキストになるかについて
の詳細は :ref:`traversal_chapter` と :ref:`urldispatch_chapter` を参照
してください。


.. The APIs provided by :ref:`traversal_module` are used against resource
.. objects.  These functions can be used to find the "path" of a resource, the
.. root resource in a resource tree, or to generate a URL for a resource.

:ref:`traversal_module` によって提供される API は、リソースオブジェクト
に対して使用されます。これらの関数は、リソースの「パス」やリソースツリー
の root リソースを見つけるために、あるいはリソース用の URL を生成するた
めに使用することができます。


.. The APIs provided by :ref:`location_module` are used against resources.
.. These can be used to walk down a resource tree, or conveniently locate one
.. resource "inside" another.

:ref:`location_module` によって提供される API は、リソースに対して使用
されます。これらは、リソースツリーを下へたどるか、あるいは別のリソース
の「内部」のリソースを簡単に見つけるために使用することができます。


.. Some APIs in :ref:`security_module` accept a resource object as a parameter.
.. For example, the :func:`~pyramid.security.has_permission` API accepts a
.. resource object as one of its arguments; the ACL is obtained from this
.. resource or one of its ancestors.  Other APIs in the :mod:`pyramid.security`
.. module also accept :term:`context` as an argument, and a context is always a
.. resource.

:ref:`security_module` 中のいくつかの API は、パラメータとしてリソース
オブジェクトを受け取ります。例えば
:func:`~pyramid.security.has_permission` API は引数の 1 つとしてリソース
オブジェクトを受け取ります; ACL がこのリソースあるいはその先祖のうちの
1 つから得られます。 :mod:`pyramid.security` モジュール中の他の API も
引数として :term:`context` を受け取ります。そしてコンテキストは常に
リソースです。
