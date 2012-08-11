.. index::
   single: application configuration

.. Application Configuration 

.. _configuration_narr:

アプリケーション設定
=========================

.. Most people already understand "configuration" as settings that influence the
.. operation of an application.  For instance, it's easy to think of the values
.. in a ``.ini`` file parsed at application startup time as "configuration".
.. However, if you're reasonably open-minded, it's easy to think of *code* as
.. configuration too.  Since Pyramid, like most other web application platforms,
.. is a *framework*, it calls into code that you write (as opposed to a
.. *library*, which is code that exists purely for you to call).  The act of
.. plugging application code that you've written into :app:`Pyramid` is also
.. referred to within this documentation as "configuration"; you are configuring
.. :app:`Pyramid` to call the code that makes up your application.

ほとんどの人は、既にアプリケーションの操作に影響を及ぼす設定項目として
「設定 (configuration)」 を理解しています。例えば、アプリケーションの
スタートアップ時に解析された ``.ini`` ファイル中の値を「設定」と
考えることは簡単です。しかし、十分に広い心を持っていれば、容易に
*コード* もまた設定と見なすことができます。Pyramid は、他のほとんどの
ウェブアプリケーションプラットフォーム同様 *フレームワーク* なので、
あなたが書いたコードを呼び出します (これと対照的なのが *ライブラリ* で、
それは純粋にあなたが呼び出すために存在するコードです)。 :app:`Pyramid`
に対してプラグインアプリケーションコードを書く行為も、この
ドキュメンテーション内では「設定」と呼ばれます; あなたはアプリケーション
を構成するコードを呼び出すように :app:`Pyramid` を設定しています。


.. There are two ways to configure a :app:`Pyramid` application:
.. :term:`imperative configuration` and :term:`declarative configuration`.  Both
.. are described below.

:app:`Pyramid` アプリケーションを設定するために2つの方法があります:
:term:`imperative configuration` と :term:`declarative configuration` です。
その両方が以下で説明されます。


.. index::
   single: imperative configuration

.. Imperative Configuration

.. _imperative_configuration:

命令型の設定
------------------------

.. "Imperative configuration" just means configuration done by Python
.. statements, one after the next.  Here's one of the simplest :app:`Pyramid`
.. applications, configured imperatively:

「命令型の設定」は、単に Python の文によって次々に行われた設定を意味します。
これは、命令型で設定された最も単純な :app:`Pyramid` アプリケーションの1つです:


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


.. We won't talk much about what this application does yet.  Just note that the
.. "configuration' statements take place underneath the ``if __name__ ==
.. '__main__':`` stanza in the form of method calls on a :term:`Configurator`
.. object (e.g. ``config.add_view(...)``).  These statements take place one
.. after the other, and are executed in order, so the full power of Python,
.. including conditionals, can be employed in this mode of configuration.

このアプリケーションが何を行うかに関しては、もうあまり詳しく説明しません。
「設定」文が ``if __name__ == '__main__':`` 断片の真下にあり、
:term:`Configurator` オブジェクトのメソッド呼び出し
(例 ``config.add_view(...)``) の形で起こることだけに注目してください。
これらの文は次々と置かれ、順番に実行されます。したがって、この設定モード
では、条件式を含む Python の完全な力を使うことができます。


.. index::
   single: view_config
   single: configuration decoration
   single: code scanning

.. Declarative Configuration

.. _decorations_and_code_scanning:

宣言型の設定
-------------------------

.. It's sometimes painful to have all configuration done by imperative code,
.. because often the code for a single application may live in many files.  If
.. the configuration is centralized in one place, you'll need to have at least
.. two files open at once to see the "big picture": the file that represents the
.. configuration, and the file that contains the implementation objects
.. referenced by the configuration.  To avoid this, :app:`Pyramid` allows you to
.. insert :term:`configuration decoration` statements very close to code that is
.. referred to by the declaration itself.  For example:

単一のアプリケーションのためのコードはしばしば多くのファイルに存在
するので、命令型のコードによってすべての設定を行うことは時々苦痛を伴います。
設定が一カ所に集中して集められていると、「全体像」を見るために少なくとも
同時に 2 つのファイルを開く必要があるでしょう: 設定を表わすファイルと、
設定によって参照されている実装オブジェクトを含むファイルです。これを
回避するために、 :app:`Pyramid` では宣言自体によって参照されるコードの
非常に近くに :term:`configuration decoration` 文を挿入することが可能です。
例えば:


.. code-block:: python
   :linenos:

   from pyramid.response import Response
   from pyramid.view import view_config

   @view_config(name='hello', request_method='GET')
   def hello(request):
       return Response('Hello')

.. The mere existence of configuration decoration doesn't cause any
.. configuration registration to be performed.  Before it has any effect on the
.. configuration of a :app:`Pyramid` application, a configuration decoration
.. within application code must be found through a process known as a
.. :term:`scan`.

設定デコレーションが単に存在するだけでは、設定登録は行なわれません。
:app:`Pyramid` アプリケーションの設定が効果も持つ前に、アプリケーション
コード内の設定デコレーションは :term:`scan` として知られているプロセス
を通じて見つけられなければなりません。


.. For example, the :class:`pyramid.view.view_config` decorator in the code
.. example above adds an attribute to the ``hello`` function, making it
.. available for a :term:`scan` to find it later.

例えば、上記のコード例における :class:`pyramid.view.view_config` デコレータは、
:term:`scan` が後でそれを見つけられるように ``hello`` 関数に属性を加えます。


.. A :term:`scan` of a :term:`module` or a :term:`package` and its subpackages
.. for decorations happens when the :meth:`pyramid.config.Configurator.scan`
.. method is invoked: scanning implies searching for configuration declarations
.. in a package and its subpackages.  For example:

:term:`module` あるいは :term:`package` およびそのサブパッケージの
デコレータに対する :term:`scan` が、 :meth:`pyramid.config.Configurator.scan`
メソッドが起動された時に起こります: scan は、パッケージおよびそのサブ
パッケージ中の設定宣言の検索を意味しています。例えば:


.. topic:: Starting A Scan

   .. code-block:: python
      :linenos:

      from wsgiref.simple_server import make_server
      from pyramid.config import Configurator
      from pyramid.response import Response
      from pyramid.view import view_config
     
      @view_config()
      def hello(request):
          return Response('Hello')

      if __name__ == '__main__':
          from pyramid.config import Configurator
          config = Configurator()
          config.scan()
          app = config.make_wsgi_app()
          server = make_server('0.0.0.0', 8080, app)
          server.serve_forever()


.. The scanning machinery imports each module and subpackage in a package or
.. module recursively, looking for special attributes attached to objects
.. defined within a module.  These special attributes are typically attached to
.. code via the use of a :term:`decorator`.  For example, the
.. :class:`~pyramid.view.view_config` decorator can be attached to a function or
.. instance method.

scan 機構は、モジュール内に定義されたオブジェクトに取り付けられた
特殊属性を探して、パッケージまたはモジュールに含まれる各モジュールと
サブパッケージを再帰的にインポートします。これらの特殊属性は典型的に
:term:`decorator` を使用してコードに取り付けられます。例えば
:class:`~pyramid.view.view_config` デコレータを関数またはインスタンス
メソッドに取り付けることができます。


.. Once scanning is invoked, and :term:`configuration decoration` is found by
.. the scanner, a set of calls are made to a :term:`Configurator` on your
.. behalf: these calls replace the need to add imperative configuration
.. statements that don't live near the code being configured.

一旦 scan が起動され、 :term:`configuration decoration` がスキャナに
よって見つかれば、 :term:`Configurator` に対する一連の呼び出しが
自動的に行われます: これらの呼び出しは、設定対象のコードから離れた
場所に命令型の設定命令を追加する必要性を置き換えます。


.. The combination of :term:`configuration decoration` and the invocation of a
.. :term:`scan` is collectively known as :term:`declarative configuration`.

:term:`configuration decoration` と :term:`scan` 起動の組み合わせは、
まとめて :term:`declarative configuration` として知られています。


.. In the example above, the scanner translates the arguments to
.. :class:`~pyramid.view.view_config` into a call to the
.. :meth:`pyramid.config.Configurator.add_view` method, effectively:

上記の例において、スキャナは実質的に :meth:`pyramid.view.view_config` に
対する引数を :class:`~pyramid.config.Configurator.add_view` メソッドの
呼び出しに変換します:


.. ignore-next-block
.. code-block:: python
   :linenos:

   config.add_view(hello)


.. Summary

まとめ
-------

.. There are two ways to configure a :app:`Pyramid` application: declaratively
.. and imperatively.  You can choose the mode you're most comfortable with; both
.. are completely equivalent.  Examples in this documentation will use both
.. modes interchangeably.

:app:`Pyramid` アプリケーションを設定するために、宣言型と命令型という
2つの方法があります。どちらでも好きなモードを選ぶことができます;
両者は完全に等価です。このドキュメンテーション中の例では、両方のモードを
交互に使用します。
