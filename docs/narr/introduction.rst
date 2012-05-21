.. index::
   single: Agendaless Consulting
   single: Pylons
   single: Django
   single: Zope
   single: frameworks vs. libraries
   single: framework


.. :app:`Pyramid` Introduction

:app:`Pyramid` イントロダクション
=================================

.. :app:`Pyramid` is a general, open source, Python web application development
.. *framework*. Its primary goal is to make it easier for a Python developer to
.. create web applications.

:app:`Pyramid` は、汎用的なオープンソースの Python ウェブアプリケーション
開発 *フレームワーク* です。その主目的は Python 開発者がウェブアプリケー
ションを作成することをより簡単にすることです。


.. sidebar:: Frameworks vs. Libraries

   .. A *framework* differs from a *library* in one very important way:
   .. library code is always *called* by code that you write, while a
   .. framework always *calls* code that you write.  Using a set of
   .. libraries to create an application is usually easier than using a
   .. framework initially, because you can choose to cede control to
   .. library code you have not authored very selectively. But when you
   .. use a framework, you are required to cede a greater portion of
   .. control to code you have not authored: code that resides in the
   .. framework itself.  You needn't use a framework at all to create a
   .. web application using Python.  A rich set of libraries already
   .. exists for the platform.  In practice, however, using a framework
   .. to create an application is often more practical than rolling your
   .. own via a set of libraries if the framework provides a set of
   .. facilities that fits your application requirements.

   *フレームワーク* は1つの非常に重要な点において *ライブラリ* とは異な
   ります: ライブラリコードは常に、あなたが書いたコードから *呼ばれ* ます。
   一方、フレームワークは常に、あなたが書いたコードを *呼び出し* ます。
   アプリケーションを作成するために1セットのライブラリを使用することは、
   初期段階ではフレームワークを使用するよりも通常は簡単です。なぜなら、
   自分の書いたものでないライブラリコードにコントロールを譲ることに対し
   て極めて選択的に決めることができるからです。しかし、フレームワークを
   使用する場合、自分の書いたものでないコード、つまりフレームワーク自体
   に存在するコードにコントロールのより大きな部分を譲ることが求められます。
   Python を使用してウェブアプリケーションを作成するために、フレームワーク
   を使用する必要は全くありません。既にそのプラットフォーム向けに豊富な
   ライブラリのセットが存在しています。しかし、フレームワークがアプリケー
   ション要件に適合する機能セットを提供している場合、実際にはライブラリ
   を組み合わせて自作するより、アプリケーションを作成するためにフレーム
   ワークを使用することは多くの場合により現実的です。



.. Pyramid attempts to follow these design and engineering principles:

Pyramid は、これらの設計とエンジニアリングの原則に従うよう努めます:


.. Simplicity

簡潔性


  .. :app:`Pyramid` takes a *"pay only for what you eat"* approach.  You can get
  .. results even if you have only a partial understanding of :app:`Pyramid`.
  .. It doesn’t force you to use any particular technology to produce an
  .. application, and we try to keep the core set of concepts that you need to
  .. understand to a minimum.

  :app:`Pyramid` は、 *「使った分だけお支払い」* アプローチを取ります。
  :app:`Pyramid` について部分的な理解だけしか持っていなくても結果を得る
  ことができます。 Pyramid はアプリケーションを生成するために特別な技術
  の使用を強制しません。私たちは、理解する必要のある概念のコアセットを
  最小に維持しようとしています。


.. Minimalism

ミニマリズム


  .. :app:`Pyramid` tries to solve only the fundamental problems of creating
  .. a web application: the mapping of URLs to code, templating, security and
  .. serving static assets. We consider these to be the core activities that are
  .. common to nearly all web applications.

  :app:`Pyramid` は、ウェブアプリケーション作成の根本問題だけを解決しよ
  うとします: URL からコードへのマッピング、テンプレート、セキュリティ、
  静的 asset の表示。私たちは、これらをほぼすべてのウェブアプリケーション
  に共通のコア活動と考えます。


.. Documentation

ドキュメンテーション


  .. Pyramid's minimalism means that it is easier for us to maintain complete
  .. and up-to-date documentation. It is our goal that no aspect of Pyramid
  .. is undocumented.

  Pyramid のミニマリズムは、完全で最新のドキュメンテーションを維持する
  ことがより簡単であることを意味します。私たちのゴールは、 Pyramid の
  どの側面も文書化されている、という状態です。


.. Speed

スピード


  .. :app:`Pyramid` is designed to provide noticeably fast execution for common
  .. tasks such as templating and simple response generation. Although "hardware
  .. is cheap", the limits of this approach become painfully evident when one
  .. finds him or herself responsible for managing a great many machines.

  :app:`Pyramid` は、テンプレート処理や単純なレスポンス生成のような共通
  タスクのために、顕著に高速な実行速度を提供することを目指しています。
  確かに「ハードウェアは安い」ですが、非常に多数のマシンを管理する責任
  を持つことになると、このアプローチの限界はひどく明らかになります。


.. Reliability

信頼性


  .. :app:`Pyramid` is developed conservatively and tested exhaustively. Where
  .. Pyramid source code is concerned, our motto is: "If it ain’t tested, it’s
  .. broke".

  :app:`Pyramid` は保守的に開発されており、徹底的にテストされています。
  Pyramid のソースコードに関して、私たちのモットーは次の通りです:
  「テストされていないものは壊れている」


.. Openness

オープン性


  .. As with Python, the Pyramid software is distributed under a `permissive
  .. open source license <http://repoze.org/license.html>`_.

  Python と同じように、 Pyramid ソフトウェアは `寛容なオープンソース
  ライセンス <http://repoze.org/license.html>`_ の下で配布されます。


.. What Makes Pyramid Unique

.. _what_makes_pyramid_unique:

Pyramid のユニークな点
-------------------------

.. Understandably, people don't usually want to hear about squishy engineering
.. principles, they want to hear about concrete stuff that solves their
.. problems.  With that in mind, what would make someone want to use Pyramid
.. instead of one of the many other web frameworks available today?  What makes
.. Pyramid unique?

当然ながら、人々は通常 squishy (感傷的) なエンジニアリング原則が聞きたいのではなく、
自分たちの問題を解決する具体的な事柄に関して聞くことを望みます。それを
念頭において、なぜ今日利用可能な他の多くのウェブフレームワークに代わって
Pyramid を使用するのでしょうか。Pyramid のユニークな点は何でしょうか。


.. This is a hard question to answer, because there are lots of excellent
.. choices, and it's actually quite hard to make a wrong choice, particularly in
.. the Python web framework market.  But one reasonable answer is this: you can
.. write very small applications in Pyramid without needing to know a lot.
.. "What?", you say, "that can't possibly be a unique feature, lots of other web
.. frameworks let you do that!"  Well, you're right.  But unlike many other
.. systems, you can also write very large applications in Pyramid if you learn a
.. little more about it.  Pyramid will allow you to become productive quickly,
.. and will grow with you; it won't hold you back when your application is small
.. and it won't get in your way when your application becomes large.  "Well
.. that's fine," you say, "lots of other frameworks let me write large apps
.. too."  Absolutely.  But other Python web frameworks don't seamlessly let you
.. do both.  They seem to fall into two non-overlapping categories: frameworks
.. for "small apps" and frameworks for "big apps".  The "small app" frameworks
.. typically sacrifice "big app" features, and vice versa.

これは答えることが困難な質問です。多くの優れた選択肢があり、特に Python
ウェブフレームワーク業界では、間違った選択を行なうことは実際にまったく
困難なためです。しかし、1つの合理的な回答がこれです: Pyramid をあまり
よく知らなくても、非常に小さなアプリケーションを書くことができます。
「え?」とあなたは言うでしょう。「それは別にユニークな特徴なんかじゃない。
他の多くのウェブフレームワークでもできるよ!」ええ、それは正しいです。
しかし、他の多くのシステムと異なり、もう少し学習すれば、さらに Pyramid
で非常に大きなアプリケーションを書くことができます。 Pyramid は、あな
たが素早く生産的になることを可能にし、あなたと共に成長します;
アプリケーションが小さい場合、 Pyramid はあなたを制約しません。また、
アプリケーションが大きくなる場合、それはあなたのやり方を邪魔しません。
「なるほど、それは素晴らしい」とあなたは言います。「でも他の多くの
フレームワークでも大きなアプリケーションを書くことはできるよ」
もちろんです。しかし、他の Python ウェブフレームワークでは、両方を
シームレスに行うことができません。それらは、オーバーラップしない2つの
カテゴリに分類されるように見えます:「小さなアプリケーション」用のフレーム
ワークと「大きなアプリケーション」用のフレームワークというように。「小さなアプリ」
フレームワークは、典型的には「大きなアプリ」の特徴を犠牲にしていて、
その逆も然りです。


.. We don't think it's a universally reasonable suggestion to write "small apps"
.. in a "small framework" and "big apps" in a "big framework".  You can't really
.. know to what size every application will eventually grow.  We don't really
.. want to have to rewrite a previously small application in another framework
.. when it gets "too big".  We believe the current binary distinction between
.. frameworks for small and large applications is just false; a well-designed
.. framework should be able to be good at both.  Pyramid strives to be that kind
.. of framework.

私たちは、「小さなフレームワーク」で「小さなアプリケーション」を書き、
「大きなフレームワーク」で「大きなアプリケーション」を書く、というのは
普遍的な合理的提案ではないと思います。すべてのアプリケーションが最終的
にどれだけのサイズに成長するかを実際に知ることはできません。以前は小さな
アプリケーションだったものを、それが「大きく」なりすぎた場合に別のフレーム
ワークで書き直さなければならない、なんてことは全然望んでいません。
私たちは、大小のアプリケーションに対してフレームワークが二分されている
現状はまったく誤りであると信じます; 良く設計されたフレームワークは、
両方に対して優れているはずです。 Pyramid はその種のフレームワークになる
ように努力します。


.. To this end, Pyramid provides a set of features, that, combined, are unique
.. amongst Python web frameworks.  Lots of other frameworks contain some
.. combination of these features; Pyramid of course actually stole many of them
.. from those other frameworks.  But Pyramid is the only one that has all of
.. them in one place, documented appropriately, and useful a la carte without
.. necessarily paying for the entire banquet.  These are detailed below.

この目的のために、 Pyramid は1セットの特徴を提供します(それらの組み合わせは、
Python ウェブフレームワークの中でユニークです)。他の多くのフレームワークは、
これらの特徴の特定の組み合わせを含んでいます; Pyramid は、もちろんそれらの
特徴のうちの多くのものを実際に他のフレームワークから盗みました。しかし、
Pyramid はそれらすべてを適切な文書化とともに一ヶ所に持つただ一つの
フレームワークです。それはフルコースに対する必然的な支払いを必要としない
便利なアラカルト (一品料理) です。これらは以下に詳述されます。


.. Single-file applications

単一ファイルアプリケーション
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. You can write a Pyramid application that lives entirely in one Python file,
.. not unlike existing Python microframeworks.  This is beneficial for one-off
.. prototyping, bug reproduction, and very small applications.  These
.. applications are easy to understand because all the information about the
.. application lives in a single place, and you can deploy them without needing
.. to understand much about Python distributions and packaging.  Pyramid isn't
.. really marketed as a microframework, but it allows you to do almost
.. everything that frameworks that are marketed as micro offer in very similar
.. ways.

既存の Python マイクロフレームワークと同じく、単一の Python ファイルの
中だけで完結する Pyramid アプリケーションを書くことができます。これは、
1回限りのプロトタイピングや、バグの再現、非常に小さなアプリケーション
には有益です。アプリケーションに関するすべての情報が一つの場所にまとまって
いるので、これらのアプリケーションは理解するのが簡単です。また、
Python の配布やパッケージングに関して多くのことを理解する必要なしに
それらをデプロイすることができます。実際のところ Pyramid はマイクロフレーム
ワークとして世に出されているわけではありませんが、マイクロフレームワークと
呼ばれているような他のフレームワークが提供することはほとんどすべて、
良く似た方法で実現できます。


.. literalinclude:: helloworld.py


.. See also :ref:`firstapp_chapter`.

:ref:`firstapp_chapter` も参照してください。


.. Decorator-based configuration

デコレータベースの設定
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. If you like the idea of framework configuration statements living next to the
.. code it configures, so you don't have to constantly switch between files to
.. refer to framework configuration when adding new code, you can use Pyramid
.. decorators to localize the configuration.  For example:

フレームワークの設定が関連するコードのすぐ近くにあり、したがって新しい
コードを追加する場合にフレームワーク設定を参照するためにファイル間で絶えず
切り替える必要がない、というアイデアが好きなら、設定を局所化するために
Pyramid デコレータを使用することができます。例えば:


.. code-block:: python

   from pyramid.view import view_config
   from pyramid.response import Response

   @view_config(route_name='fred')
   def fred_view(request):
       return Response('fred')


.. However, unlike some other systems, using decorators for Pyramid
.. configuration does not make your application difficult to extend, test or
.. reuse.  The :class:`~pyramid.view.view_config` decorator, for example, does
.. not actually *change* the input or output of the function it decorates, so
.. testing it is a "WYSIWYG" operation; you don't need to understand the
.. framework to test your own code, you just behave as if the decorator is not
.. there.  You can also instruct Pyramid to ignore some decorators, or use
.. completely imperative configuration instead of decorators to add views.
.. Pyramid decorators are inert instead of eager: you detect and activate them
.. with a :term:`scan`.

しかし、他のいくつかのシステムとは異なり、 Pyramid 設定にデコレータを
利用することはアプリケーションの拡張、テスト、再利用を困難にしません。
例えば、 :class:`~pyramid.view.view_config` デコレータは、デコレートする
関数の入出力を実際には *変更* しません。したがって、そのテストは
「WYSIWYG」な操作です; 自分のコードをテストするためにフレームワークを
理解する必要はありません。単にあたかもそこにデコレータが存在しないかの
ように振る舞えば良いのです。さらに、 Pyramid がいくつかのデコレータを無視
するようにしたり、ビューを追加するためにデコレータの代わりに完全に命令
的な設定を使用することもできます。 Pyramid デコレータは積極的 (eager)
ではなく消極的 (inert) です。 :term:`scan` によってそれらを検知し活性化
します。


.. Example: :ref:`mapping_views_using_a_decorator_section`.

例: :ref:`mapping_views_using_a_decorator_section`


.. URL generation

URL 生成
~~~~~~~~~~~~~~

.. Pyramid is capable of generating URLs for resources, routes, and static
.. assets.  Its URL generation APIs are easy to use and flexible.  If you use
.. Pyramid's various APIs for generating URLs, you can change your configuration
.. around arbitrarily without fear of breaking a link on one of your web pages.

Pyramid は、リソース、ルートおよび静的 asset 用に URL を生成することが
できます。その URL 生成 API は使いやすく柔軟です。 URL を生成するために
Pyramid の様々な API を使用すれば、多数のウェブページ中で一つのリンク
切れも起こす心配なしに設定を任意に変更することができます。


.. Example: :ref:`generating_route_urls`.

例: :ref:`generating_route_urls`


.. Static file serving

静的ファイルサーバ
~~~~~~~~~~~~~~~~~~~

.. Pyramid is perfectly willing to serve static files itself.  It won't make you
.. use some external web server to do that.  You can even serve more than one
.. set of static files in a single Pyramid web application (e.g. ``/static`` and
.. ``/static2``).  You can also, optionally, place your files on an external web
.. server and ask Pyramid to help you generate URLs to those files, so you can
.. use Pyramid's internal fileserving while doing development, and a faster
.. static file server in production without changing any code.

Pyramid はそれ自体で静的ファイルを返す完全な機能を持ちます。そのため
外部のウェブサーバを使用する必要はありません。さらに、 1 つの Pyramid
ウェブアプリケーションで複数の静的ファイルの集合を返すことができます
(例えば ``/static`` と ``/static2``)。さらに、任意で、ファイルを外部
ウェブサーバに置き、 Pyramid がそれらのファイルへの URL を生成するように
できます。したがって、コードも一切変更することなく、開発時には Pyramid
の内部ファイルサーバを使い、プロダクション環境ではより高速な静的ファイル
サーバを使用することができます。


.. Example: :ref:`static_assets_section`.

例: :ref:`static_assets_section`


.. Debug Toolbar

デバッグツールバー
~~~~~~~~~~~~~~~~~~

.. Pyramid's debug toolbar comes activated when you use a Pyramid scaffold to
.. render a project.  This toolbar overlays your application in the browser, and
.. allows you access to framework data such as the routes configured, the last
.. renderings performed, the current set of packages installed, SQLAlchemy
.. queries run, logging data, and various other facts.  When an exception
.. occurs, you can use its interactive debugger to poke around right in your
.. browser to try to determine the cause of the exception.  It's handy.

Pyramid scaffold を使用した場合、 Pyramid のデバッグツールバーが有効に
なった状態でプロジェクトが作成されます。このツールバーは、ブラウザ内で
アプリケーションを覆って、フレームワークデータへのアクセスを与え
ます: 設定されたルート、最後のレンダリング結果、現在インストールされて
いるパッケージ一覧、 SQLAlchemy クエリ実行、ログデータ、および他の
様々な情報。例外が生じた場合、例外の原因を特定するためにブラウザの中で
色々とつつき回るために対話型デバッガを使用することができます。それは手軽です。


.. Example: :ref:`debug_toolbar`.

例: :ref:`debug_toolbar`


.. Debugging settings

デバッグ設定
~~~~~~~~~~~~~~~~~~

.. Pyramid has debugging settings that allow you to print Pyramid runtime
.. information to the console when things aren't behaving as you're expecting.
.. For example, you can turn on "debug_notfound", which prints an informative
.. message to the console every time a URL does not match any view.  You can
.. turn on "debug_authorization", which lets you know why a view execution was
.. allowed or denied by printing a message to the console.  These features are
.. useful for those WTF moments.

Pyramid にはデバッグ設定があり、物事が期待した通りに動作していない場合
に Pyramid ランタイム情報をコンソールに表示することができます。例えば、
"debug_notfound" を有効にすると、 URL がビューと一致しない場合はいつで
もコンソールに通知メッセージを表示するようになります。
"debug_authorization" を有効にすることで、なぜビュー実行が許可または禁
止されたをコンソールにメッセージ表示することで知らせます。これらの特徴
は、これらの WTF 時に役立ちます (訳注: What the fuck = 「なんてこった」の意味?)。


.. There are also a number of commands that you can invoke within a Pyramid
.. environment that allow you to introspect the configuration of your system:
.. ``proutes`` shows all configured routes for an application in the order
.. they'll be evaluated for matching; ``pviews`` shows all configured views for
.. any given URL.  These are also WTF-crushers in some circumstances.

さらに、システムの設定を調査するために Pyramid 環境内で実行できる数々の
コマンドがあります: ``proutes`` は、アプリケーション用に設定されたルートを
マッチに際してそれらが評価される順番ですべて示します; ``pviews`` は、
所定の URL に対して設定されたビューをすべて表示します。これらはまた、
いくつかの状況で WTF-crushers (訳注: デバッグに便利、というような意味か?) です。


.. Examples: :ref:`debug_authorization_section` and :ref:`command_line_chapter`.

例: :ref:`debug_authorization_section`, :ref:`command_line_chapter`


.. Add-ons

アドオン
~~~~~~~~

.. Pyramid has an extensive set of add-ons held to the same quality standards as
.. the Pyramid core itself.  Add-ons are packages which provide functionality
.. that the Pyramid core doesn't.  Add-on packages already exist which let you
.. easily send email, let you use the Jinja2 templating system, let you use
.. XML-RPC or JSON-RPC, let you integrate with jQuery Mobile, etc.

Pyramid には、 Pyramid コアそれ自体と同じ品質水準を持つ広範囲なアドオン
のセットがあります。アドオンは Pyramid コアが提供しない機能を提供する
パッケージです。電子メールを簡単に送ったり、Jinja2テンプレートシステム
を使用したり、 XML-RPC あるいは JSON-RPC を使用したり、jQuery Mobile と
統合したりするアドオンパッケージは既に存在します。


.. Examples: http://docs.pylonsproject.org/docs/pyramid.html#pyramid-add-on-documentation

例: http://docs.pylonsproject.org/docs/pyramid.html#pyramid-add-on-documentation


.. Class-based and function-based views

クラスベースと関数ベースのビュー
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Pyramid has a structured, unified concept of a :term:`view callable`.
.. View callables can be functions, methods of classes, or even instances.  When
.. you add a new view callable, you can choose to make it a function or a method
.. of a class; in either case, Pyramid treats it largely the same way.  You can
.. change your mind later, and move code between methods of classes and
.. functions.  A collection of similar view callables can be attached to a
.. single class as methods, if that floats your boat, and they can share
.. initialization code as necessary.  All kinds of views are easy to understand
.. and use and operate similarly.  There is no phony distinction between them;
.. they can be used for the same purposes.

Pyramid には :term:`view callable` という構造化された統一的な概念があり
ます。ビュー callable は、関数、クラスのメソッド、あるいはインスタンス
です。新しいビュー callable を加える場合、それを関数あるいはクラスの
メソッドのどちらにするか選択することができます; いずれの場合も Pyramid
はそれを大部分は同じ方法で扱います。もし後で気が変わっても、クラスの
メソッドと関数の間でコードを移動させることができます。類似したビュー
callable のコレクションは、メソッドとして単一のクラスに取り付けることが
できます。好みに応じて、必要なら初期化コードを共有することもできます。
すべての種類のビューは、理解して使用するのが簡単で、かつ同様に作動します。
それらの間に偽りの区別はありません; それらは同じ目的のために使用できます。


.. Here's a view callable defined as a function:

これは関数として定義したビュー callable です:


.. code-block:: python
   :linenos:

   from pyramid.response import Response
   from pyramid.view import view_config

   @view_config(route_name='aview')
   def aview(request):
       return Response('one')


.. Here's a few views defined as methods of a class instead:

これは代わりにクラスのメソッドとして定義したビュー callable です:


.. code-block:: python
   :linenos:

   from pyramid.response import Response
   from pyramid.view import view_config

   class AView(object):
       def __init__(self, request):
           self.request = request

       @view_config(route_name='view_one')
       def view_one(self):
           return Response('one')

       @view_config(route_name='view_two')
       def view_two(self):
           return Response('two')


.. See also :ref:`view_config_placement`.

:ref:`view_config_placement` も参照してください。


.. Asset specifications

.. _intro_asset_specs:

asset 仕様
~~~~~~~~~~~~~~~~~~~~

.. Asset specifications are strings that contain both a Python package name and
.. a file or directory name, e.g. ``MyPackage:static/index.html``.  Use of these
.. specifications is omnipresent in Pyramid.  An asset specification can refer
.. to a template, a translation directory, or any other package-bound static
.. resource.  This makes a system built on Pyramid extensible, because you don't
.. have to rely on globals ("*the* static directory") or lookup schemes ("*the*
.. ordered set of template directories") to address your files.  You can move
.. files around as necessary, and include other packages that may not share your
.. system's templates or static files without encountering conflicts.

asset 仕様は、例えば ``MyPackage:static/index.html`` のように Python
パッケージ名とファイルまたはディレクトリ名の両方を含む文字列です。
この仕様は Pyramid の中のほとんどあらゆる場所で使われています。
asset 仕様は、テンプレート、翻訳ディレクトリ、あるいは他のパッケージに
結合された静的なリソースを指すことができます。これは Pyramid 上に構築された
システムを拡張可能にします。なぜなら、ファイルを指し示すためにグローバル
変数 (「static ディレクトリ」) や、検索スキーム (「テンプレートディレクトリ
の順序集合」) に頼る必要がないからです。必要に応じてファイルを移動する
ことができ、システムのテンプレートや静的ファイルを共有することができない
他のパッケージを衝突することなくインクルードすることができます。


.. Because asset specifications are used heavily in Pyramid, we've also provided
.. a way to allow users to override assets.  Say you love a system that someone
.. else has created with Pyramid but you just need to change "that one template"
.. to make it all better.  No need to fork the application.  Just override the
.. asset specification for that template with your own inside a wrapper, and
.. you're good to go.

asset 仕様は Pyramid の中で極度に使用されているので、ユーザ
が asset をオーバーライドできるようにする方法も提供されています。仮にあなたが誰か
他の人によって Pyramid で作成されたシステムを気に入って、それを改良するために
「ある1つのテンプレート」だけを変更する必要があるとします。アプリケーション
をフォークする必要はありません。ラッパー内部でそのテンプレート用の
asset 仕様をあなた自身のもので単にオーバーライドしてください。それだけ
でうまくいきます。


.. Examples: :ref:`asset_specifications` and :ref:`overriding_assets_section`.

例: :ref:`asset_specifications`, :ref:`overriding_assets_section`


.. Extensible templating

拡張可能なテンプレート
~~~~~~~~~~~~~~~~~~~~~~

.. Pyramid has a structured API that allows for pluggability of "renderers".
.. Templating systems such as Mako, Genshi, Chameleon, and Jinja2 can be treated
.. as renderers.  Renderer bindings for all of these templating systems already
.. exist for use in Pyramid.  But if you'd rather use another, it's not a big
.. deal.  Just copy the code from an existing renderer package, and plug in your
.. favorite templating system.  You'll then be able to use that templating
.. system from within Pyramid just as you'd use one of the "built-in" templating
.. systems.

Pyramid には「レンダラー」のプラグ可能性を実現するための構造化 API があります。
Mako, Genshi, Chameleon, Jinja2 のようなテンプレートシステムをレンダラー
として扱うことができます。これらすべてのテンプレートシステムのための
レンダラーバインディングは Pyramid で使用するために既に存在しています。
しかし、あなたがもし別のテンプレートシステムを使用したかったとしても、
それは大したことではありません。既存のレンダラーパッケージからコードを
単にコピーして、好きなテンプレートシステムをプラグインしてください。
そうすれば、ちょうど「内蔵の」テンプレートシステムを使用するのと同じように、
Pyramid の内部からそのテンプレートシステムを使用することが
できるようになります。


.. Pyramid does not make you use a single templating system exclusively.  You
.. can use multiple templating systems, even in the same project.

Pyramid では単一のテンプレートシステムを排他的に使用する必要はありません。
同じプロジェクトの中でも多数のテンプレートシステムを使用することができます。


.. Example: :ref:`templates_used_directly`.

例: :ref:`templates_used_directly`


.. Rendered views can return dictionaries

レンダリングされるビューは辞書を返すことができる
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. If you use a :term:`renderer`, you don't have to return a special kind of
.. "webby" ``Response`` object from a view.  Instead, you can return a
.. dictionary instead, and Pyramid will take care of converting that dictionary
.. to a Response using a template on your behalf.  This makes the view easier to
.. test, because you don't have to parse HTML in your tests; just make an
.. assertion instead that the view returns "the right stuff" in the dictionary
.. it returns.  You can write "real" unit tests instead of functionally testing
.. all of your views.

:term:`renderer` を使用していれば、ビューから特別な種類の「Web 特有」の
``Response`` オブジェクトを返す必要はありません。代わりに辞書を返すことが
できます。すると Pyramid は、代わりにテンプレートを使用してその辞書をレスポ
ンスに変換する世話をします。これはビューをテストすることをより簡単にします。
テストで HTML を解析する必要がないからです; 代わりに、単にビューが「正しい値」
を含んだ辞書を返すという assertion をしてください。あなたのビューすべてを
機能的にテストする代わりに、「本物の」ユニットテストを書くことができます。


.. For example, instead of:

例えば、こうする代わりに:


.. code-block:: python
   :linenos:

    from pyramid.renderers import render_to_response

    def myview(request):
        return render_to_response('myapp:templates/mytemplate.pt', {'a':1},
                                  request=request)


.. You can do this:

このようにすることができます:


.. code-block:: python
   :linenos:

    from pyramid.view import view_config

    @view_config(renderer='myapp:templates/mytemplate.pt')
    def myview(request):
        return {'a':1}


.. When this view callable is called by Pyramid, the ``{'a':1}`` dictionary will
.. be rendered to a response on your behalf.  The string passed as ``renderer=``
.. above is an :term:`asset specification`.  It is in the form
.. ``packagename:directoryname/filename.ext``.  In this case, it refers to the
.. ``mytemplate.pt`` file in the ``templates`` directory within the ``myapp``
.. Python package.  Asset specifications are omnipresent in Pyramid: see
.. :ref:`intro_asset_specs` for more information.

このビュー callable が Pyramid によって呼ばれる時、辞書 ``{'a':1}`` が
あなたの代わりにレスポンスに与えられるでしょう。上記の ``renderer=`` と
して渡された文字列は :term:`asset specification` です。それは
``packagename:directoryname/filename.ext`` の形式です。この場合、それは
``myapp`` Python パッケージ内の ``templates`` ディレクトリにある
``mytemplate.pt`` ファイルを指します。 asset 仕様は Pyramid において
広範に使用されます: 詳細は :ref:`intro_asset_specs` を参照してください。


.. Example: :ref:`renderers_chapter`.

例: :ref:`renderers_chapter`


.. Event system

イベントシステム
~~~~~~~~~~~~~~~~

.. Pyramid emits *events* during its request processing lifecycle.  You can
.. subscribe any number of listeners to these events.  For example, to be
.. notified of a new request, you can subscribe to the ``NewRequest`` event.  To
.. be notified that a template is about to be rendered, you can subscribe to the
.. ``BeforeRender`` event, and so forth.  Using an event publishing system as a
.. framework notification feature instead of hardcoded hook points tends to make
.. systems based on that framework less brittle.

Pyramid はそのリクエスト処理ライフサイクル中に *イベント* を送出します。
これらのイベントに任意の数のリスナを登録することができます。例えば、
新しいリクエストについて通知を受けるために、 ``NewRequest`` イベントに
登録することができます。テンプレートをレンダリングする直前で通知を受け
るために、 ``BeforeRender`` イベントに登録することができます。などなど。
ハードコードされたフックポイントの代わりに、フレームワーク通知機能として
イベント公開システムを使用することは、そのフレームワークに基づくシステム
をより壊れにくくする傾向があります (訳注: この文の意味は良く分からない)。


.. You can also use Pyramid's event system to send your *own* events.  For
.. example, if you'd like to create a system that is itself a framework, and may
.. want to notify subscribers that a document has just been indexed, you can
.. create your own event type (``DocumentIndexed`` perhaps) and send the event
.. via Pyramid.  Users of this framework can then subscribe to your event like
.. they'd subscribe to the events that are normally sent by Pyramid itself.

さらに、自分のイベントを送るために Pyramid のイベントシステムを使用する
ことができます。例えば、それ自体がフレームワークであるようなシステムを
構築していて、ドキュメントがたった今インデックス化されたことを受信者に
通知したければ、自分のイベントタイプ (恐らく ``DocumentIndexed``) を
作成して Pyramid 経由でイベントを送ることができます。そうすればこの
フレームワークのユーザは、 Pyramid 自体によって通常送られるイベントと
同じように、あなたのイベントに登録することができます。


.. Example: :ref:`events_chapter` and :ref:`event_types`.

例: :ref:`events_chapter`, :ref:`event_types`.


.. Built-in internationalization

組み込みの国際化機能
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Pyramid ships with internationalization-related features in its core:
.. localization, pluralization, and creating message catalogs from source files
.. and templates.  Pyramid allows for a plurality of message catalog via the use
.. of translation domains: you can create a system that has its own translations
.. without conflict with other translations in other domains.

Pyramid はコアの中に国際化関連の機能を備えています: ローカライゼーション、
複数形変換、およびソースファイルとテンプレートからのメッセージカタログの作成。
Pyramid は翻訳ドメインの使用によって複数のメッセージカタログを考慮します:
他のドメインの他の翻訳と衝突することなくそれ自身の翻訳を持つシステムを
作成することができます。


.. Example: :ref:`i18n_chapter`.

例: :ref:`i18n_chapter`


.. HTTP caching

HTTP キャッシュ
~~~~~~~~~~~~~~~

.. Pyramid provides an easy way to associate views with HTTP caching policies.
.. You can just tell Pyramid to configure your view with an ``http_cache``
.. statement, and it will take care of the rest:

Pyramid はビューと HTTP キャッシングポリシーを関連付ける簡単な方法を
提供しています。 ``http_cache`` を付けてビューを設定すれば、残りは
Pyramid が面倒を見てくれます:


::

   @view_config(http_cache=3600) # 60 minutes
   def myview(request): ....


.. Pyramid will add appropriate ``Cache-Control`` and ``Expires`` headers to
.. responses generated when this view is invoked.

Pyramid は、このビューが起動された時に生成されるレスポンスに
適切な ``Cache-Control`` および ``Expires`` ヘッダーを追加します。


.. See the :meth:`~pyramid.config.Configurator.add_view` method's
.. ``http_cache`` documentation for more information.

詳細については :meth:`~pyramid.config.Configurator.add_view` メソッドの
``http_cache`` ドキュメンテーションを参照してください。


.. Sessions

セッション
~~~~~~~~~~

.. Pyramid has built-in HTTP sessioning.  This allows you to associate data with
.. otherwise anonymous users between requests.  Lots of systems do this.  But
.. Pyramid also allows you to plug in your own sessioning system by creating
.. some code that adheres to a documented interface.  Currently there is a
.. binding package for the third-party Beaker sessioning system that does exactly
.. this.  But if you have a specialized need (perhaps you want to store your
.. session data in MongoDB), you can.  You can even switch between
.. implementations without changing your application code.

Pyramid は内蔵の HTTP セッション管理を持っています。これは、それ以外の
点では匿名のユーザにリクエスト間でデータを関連付けることを可能にします。
多くのシステムがこの機能を持っています。しかし、 Pyramid はさらに、文書化された
インターフェースに適合する多少のコードを作成することで、あなた自身
のセッション管理システムをプラグインすることができます。現在、まさに
これを行っているサードパーティの Beaker セッションシステムのための
バインディングパッケージが存在します。しかし、特別なニーズ (例えば
MongoDB にセッションデータを格納したい等) を持っていれば、それも可能です。
さらに、アプリケーションコードを変更せずにセッション管理の実装を切り替える
こともできます。


.. Example: :ref:`sessions_chapter`.

例: :ref:`sessions_chapter`


.. Speed

スピード
~~~~~~~~

.. The Pyramid core is, as far as we can tell, at least marginally faster than
.. any other existing Python web framework.  It has been engineered from the
.. ground up for speed.  It only does as much work as absolutely necessary when
.. you ask it to get a job done.  Extraneous function calls and suboptimal
.. algorithms in its core codepaths are avoided.  It is feasible to get, for
.. example, between 3500 and 4000 requests per second from a simple Pyramid view
.. on commodity dual-core laptop hardware and an appropriate WSGI server
.. (mod_wsgi or gunicorn).  In any case, performance statistics are largely
.. useless without requirements and goals, but if you need speed, Pyramid will
.. almost certainly never be your application's bottleneck; at least no more
.. than Python will be a bottleneck.

Pyramid コアは、私たちが知る限り、少なくとも他のどの既存の Python ウェブ
フレームワークよりも多少は高速です。 Pyramid は最初から速度のために
エンジニアリングされました。 Pyramid は、仕事の完了を依頼
された時に絶対的に必要な分の量の仕事だけをします。外部の関数呼び出し
や最適でないアルゴリズムは、コアコードパス中では避けられています。
例えば、デュアルコアのコモディティなラップトップハードウェアと適切な
WSGI サーバ (mod_wsgi または gunicorn) 上で、単純な Pyramid ビューで
毎秒 3500〜4000 リクエストを得ることは現実的です。いずれにしても、必要条件
とゴールなしではパフォーマンス統計は大して役に立ちませんが、
あなたが速度を必要とした場合に Pyramid はほぼ確実に
アプリケーションのボトルネックにはなりません; 少なくとも、 Python が
ボトルネックになる以上には。


.. Example: http://blog.curiasolutions.com/the-great-web-framework-shootout/

例: http://blog.curiasolutions.com/the-great-web-framework-shootout/


.. Exception views

例外ビュー
~~~~~~~~~~~~~~~

.. Exceptions happen.  Rather than deal with exceptions that might present
.. themselves to a user in production in an ad-hoc way, Pyramid allows you to
.. register an :term:`exception view`.  Exception views are like regular Pyramid
.. views, but they're only invoked when an exception "bubbles up" to Pyramid
.. itself.  For example, you might register an exception view for the
.. :exc:`Exception` exception, which will catch *all* exceptions, and present a
.. pretty "well, this is embarrassing" page.  Or you might choose to register an
.. exception view for only specific kinds of application-specific exceptions,
.. such as an exception that happens when a file is not found, or an exception
.. that happens when an action cannot be performed because the user doesn't have
.. permission to do something.  In the former case, you can show a pretty "Not
.. Found" page; in the latter case you might show a login form.

例外は起こります。プロダクション環境でユーザに表示される可能性のある
例外をアドホックな方法で扱うのではなく、 Pyramid は :term:`exception
view` を登録することができます。例外ビューは通常の Pyramid ビューに
似ていますが、例外が Pyramid 自体に「bubble up」する場合にのみ起動
されます。例えば、 :exc:`Exception` 例外に対する例外ビューを登録した
とすると、それは *すべての* 例外を捕捉して、きれいな「おや、これは困った」
ページを表示します。あるいは、アプリケーションに特有の特定の種類の例外
だけに対する例外ビューを登録することもできます。例えばファイルが
見つからない場合に起こる例外や、またはユーザが何かをする許可を持たない
ためにアクションを行なうことができない場合に起こる例外などです。
前者の場合、きれいな「Not Found」ページを表示することができます;
後者の場合には、ログインフォームを表示するかもしれません。


.. Example: :ref:`exception_views`.

例: :ref:`exception_views`


.. No singletons

シングルトン不要
~~~~~~~~~~~~~~~~

.. Pyramid is written in such a way that it requires your application to have
.. exactly zero "singleton" data structures.  Or, put another way, Pyramid
.. doesn't require you to construct any "mutable globals".  Or put even a
.. different way, an import of a Pyramid application needn't have any
.. "import-time side effects".  This is esoteric-sounding, but if you've ever
.. tried to cope with parameterizing a Django "settings.py" file for multiple
.. installations of the same application, or if you've ever needed to
.. monkey-patch some framework fixture so that it behaves properly for your use
.. case, or if you've ever wanted to deploy your system using an asynchronous
.. server, you'll end up appreciating this feature.  It just won't be a problem.
.. You can even run multiple copies of a similar but not identically configured
.. Pyramid application within the same Python process.  This is good for shared
.. hosting environments, where RAM is at a premium.

Pyramid は、「シングルトン」データ構造を正確に一つも持たないことを
アプリケーションに要求するような方法で書かれています。あるいは、別の言い方を
すれば Pyramid は「書き換え可能なグローバル変数」を構築することを一切
要求しません。また別の言い方をすれば、 Pyramid アプリケーションのインポートは
「インポート時の副作用」を持ちません。これは深淵なテーマの
ように聞こえますが、あなたがこれまでに Django の "settings.py" ファイル
を同じアプリケーションの複数のインストールに対してパラメータ化しようとしたり、
あるフレームワーク内で固定的に書かれたコードをあなたのユースケースに対して適切
に動作させるためにモンキーパッチを当てる必要があったり、システムをデプロイ
するために非同期サーバーを使用しようとしたことがあれば、最終的に
この特徴を評価するでしょう。それはまったく問題になりません。さらに、
ある Pyramid アプリケーションの類似しているけれど同じではない設定の
複数のコピーを同じ Python プロセス内で実行することができます。これは
共有ホスティング環境には適しています (そのような環境で RAM は貴重です)。


.. View predicates and many views per route

ビュー述語と、1つのルートに複数のビュー
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Unlike many other systems, Pyramid allows you to associate more than one view
.. per route.  For example, you can create a route with the pattern ``/items``
.. and when the route is matched, you can shuffle off the request to one view if
.. the request method is GET, another view if the request method is POST, etc.
.. A system known as "view predicates" allows for this.  Request method matching
.. is the very most basic thing you can do with a view predicate.  You can also
.. associate views with other request parameters such as the elements in the
.. query string, the Accept header, whether the request is an XHR request or
.. not, and lots of other things.  This feature allows you to keep your
.. individual views "clean"; they won't need much conditional logic, so they'll
.. be easier to test.

他の多くのシステムと異なり、 Pyramid は1つのルートに複数のビューを関連
付けることができます。例えば、パターン ``/items`` に対してルートを作り、
ルートがマッチしたときに、リクエストメソッドが GET だったらリクエストを
あるビューにディスパッチし、リクエストメソッドが POST だったら別の
ビューにディスパッチするといったことができます。「ビュー述語」として知られ
ているシステムがこれを実現します。リクエストメソッドのマッチはビュー述語で
できる最も基礎的なことです。さらに、クエリ文字列中の要素、 Accept
ヘッダー、そのリクエストが XHR リクエストかどうか、あるいはその他多くの
リクエストパラメータにビューを関連付けることができます。この特徴によって、
個々のビューを「クリーン」に保つことができます; それらはあまり条件付きの
ロジックを必要としないでしょう。したがって、それらはより簡単にテストする
ことができます。


.. Example: :ref:`view_configuration_parameters`.

例: :ref:`view_configuration_parameters`


.. Transaction management

トランザクション管理
~~~~~~~~~~~~~~~~~~~~~~

.. Pyramid's :term:`scaffold` system renders projects that include a
.. *transaction management* system, stolen from Zope.  When you use this
.. transaction management system, you cease being responsible for committing
.. your data anymore.  Instead, Pyramid takes care of committing: it commits at
.. the end of a request or aborts if there's an exception.  Why is that a good
.. thing?  Having a centralized place for transaction management is a great
.. thing.  If, instead of managing your transactions in a centralized place, you
.. sprinkle ``session.commit`` calls in your application logic itself, you can
.. wind up in a bad place.  Wherever you manually commit data to your database,
.. it's likely that some of your other code is going to run *after* your commit.
.. If that code goes on to do other important things after that commit, and an
.. error happens in the later code, you can easily wind up with inconsistent
.. data if you're not extremely careful.  Some data will have been written to
.. the database that probably should not have.  Having a centralized commit
.. point saves you from needing to think about this; it's great for lazy people
.. who also care about data integrity.  Either the request completes
.. successfully, and all changes are committed, or it does not, and all changes
.. are aborted.

Pyramid の scaffold システムは、 Zope から取られた *トランザクション
管理* システムを含むプロジェクトを生成します。このトランザクション管理
システムを使用すると、あなたはもはやデータをコミットすることに責任を
持たなくなります。代わりに Pyramid がコミットの面倒を見ます:
それはリクエストの終わりにコミットするか、あるいは例外がある場合に
アボートします。なぜそれが良いものなのでしょうか。トランザクション管理
のための中心的な場所を持つことは素晴らしいことです。もし、中心的な場所
でトランザクションを管理する代わりに、アプリケーションロジック自体の中に
``session.commit`` 呼び出しをまき散らせば、悪い結果に行き着くでしょう。
データベースに手動でデータをコミットしているところならどこでも、他の
コードのうちの一部はそのコミットの *後で* 実行されます。コミットした
後でコードが他の重要なことをして、後のコードの中でエラーが起こる場合、
非常に注意深くしなければ、容易にデータの不整合を引き起こします。恐らく
書き出されるべきでないデータがデータベースに書かれているでしょう。中心的な
コミットポイントを持っていることで、これに関して考える必要がなくなります;
それは、さらにデータの一貫性に関心がある怠惰な人々にとっても素晴らしい
ものです。リクエストは成功裡に完了して変更がすべてコミットされるか、
コミットされずにすべての変更がアボートされるかのいずれかです。


.. Also, Pyramid's transaction management system allows you to synchronize
.. commits between multiple databases, and allows you to do things like
.. conditionally send email if a transaction commits, but otherwise keep quiet.

さらに、 Pyramid のトランザクション管理システムは複数のデータベース間で
コミットを同期することを可能にします。そして、トランザクションがコミット
する場合に条件付きで電子メールを送り、そうでなければ何もしないといった
ことができるようになります。


.. Example: :ref:`bfg_sql_wiki_tutorial` (note the lack of commit statements
.. anywhere in application code).

例: :ref:`bfg_sql_wiki_tutorial` (アプリケーションコードのどこにも
コミット文がないことに注目してください)。


.. Configuration conflict detection

矛盾する設定の検知
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. When a system is small, it's reasonably easy to keep it all in your head.
.. But when systems grow large, you may have hundreds or thousands of
.. configuration statements which add a view, add a route, and so forth.
.. Pyramid's configuration system keeps track of your configuration statements,
.. and if you accidentally add two that are identical, or Pyramid can't make
.. sense out of what it would mean to have both statements active at the same
.. time, it will complain loudly at startup time.  It's not dumb though: it will
.. automatically resolve conflicting configuration statements on its own if you
.. use the configuration :meth:`~pyramid.config.Configurator.include` system:
.. "more local" statements are preferred over "less local" ones.  This allows
.. you to intelligently factor large systems into smaller ones.

システムが小さいうちは、それをすべて頭の中に入れておくことはそれほど
難しくありません。しかし、システムが大きくなると、ビューの追加やルートの
追加を行う設定ステートメントが何百、あるいは何千になることがあります。
Pyramid の設定システムは、あなたの設定ステートメントを追跡します。
そして、同一の設定を誤って何度も追加しようとしたり、両方のステートメント
を同時に有効にすることの意味を Pyramid が理解できない場合、スタートアップ
時に派手に文句を言うでしょう。しかしながら、それは愚かではありません:
設定 :meth:`~pyramid.config.Configurator.include` システムを使えば、
矛盾する設定ステートメントを自動的に解決してくれます:
「ローカルな」ステートメントは「ローカルでない」ものより優先されます。
これは、賢い方法で大規模なシステムをより小さなシステムの要素として
含めることを可能にします。


.. Example: :ref:`conflict_detection`.

例: :ref:`conflict_detection`


.. Configuration extensibility

設定の拡張可能性
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Unlike other systems, Pyramid provides a structured "include" mechanism (see
.. :meth:`~pyramid.config.Configurator.include`) that allows you to compose
.. applications from multiple Python packages.  All the configuration statements
.. that can be performed in your "main" Pyramid application can also be
.. performed by included packages including the addition of views, routes,
.. subscribers, and even authentication and authorization policies. You can even
.. extend or override an existing application by including another application's
.. configuration in your own, overriding or adding new views and routes to
.. it.  This has the potential to allow you to compose a big application out of
.. many other smaller ones.  For example, if you want to reuse an existing
.. application that already has a bunch of routes, you can just use the
.. ``include`` statement with a ``route_prefix``; the new application will live
.. within your application at a URL prefix.  It's not a big deal, and requires
.. little up-front engineering effort.

他のシステムと異なり、 Pyramid は構造化された「インクルード」メカニズムを
提供します (:meth:`~pyramid.config.Configurator.include` を参照)。
それは複数の Python パッケージからアプリケーションを構成することを可能
にします。ビューやルート、 (イベントの) サブスクライバー、および認証と認可ポリシーの追加など、
「メイン」 Pyramid アプリケーションで行なうことができるすべての設定
ステートメントはインクルードされたパッケージでも行なうことができます。
別のアプリケーションの設定をあなたのアプリケーションにインクルードして、
オーバーライドしたり新しいビューやルートを追加したりすることで、さらに
既存のアプリケーションを拡張またはオーバーライドすることができます。
これには、大きなアプリケーションを他の多くのより小さなアプリケーション
から構成できる可能性があります。例えば、既に沢山のルートを持つ既存の
アプリケーションを再利用したければ、単純に ``route_prefix`` を付けて
``include`` ステートメントを使用することができます; 新しいアプリケーション
はあなたのアプリケーション内の URL プリフィックスで動作するようになります。
それはたいしたことではなく、事前の技術的努力をほとんど必要としません。


.. For example:

例えば:


.. code-block:: python
   :linenos:

   from pyramid.config import Configurator

   if __name__ == '__main__':
      config = Configurator()
      config.include('pyramid_jinja2')
      config.include('pyramid_exclog')
      config.include('some.other.guys.package', route_prefix='/someotherguy')


.. See also :ref:`including_configuration` and :ref:`building_an_extensible_app`

:ref:`including_configuration` と :ref:`building_an_extensible_app` も
参照してください。


.. Flexible authentication and authorization

柔軟な認証と認可
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Pyramid includes a flexible, pluggable authentication and authorization
.. system.  No matter where your user data is stored, or what scheme you'd like
.. to use to permit your users to access your data, you can use a predefined
.. Pyramid plugpoint to plug in your custom authentication and authorization
.. code.  If you want to change these schemes later, you can just change it in
.. one place rather than everywhere in your code.  It also ships with prebuilt
.. well-tested authentication and authorization schemes out of the box.  But
.. what if you don't want to use Pyramid's built-in system?  You don't have to.
.. You can just write your own bespoke security code as you would in any other
.. system.

Pyramid は柔軟でプラグイン可能な認証と認可のシステムを含んでいます。
ユーザデータがどこに保存されるか、ユーザがデータにアクセスするのを許可
するためにどんなスキームを使用するかに関わらず、カスタムの認証と認可の
コードをプラグインするためにあらかじめ定義された Pyramid プラグポイント
を使用することができます。後になってこれらのスキームを変更したければ、
コードのあちこちではなく、たった1ヶ所を変更するだけで済みます。さらに、
Pyramid には構築済みの十分テストされた認証および認可スキームが
最初から付属しています。でも、もし万が一 Pyramid の内蔵のシステムを
使いたくないとしたら? それを使う必要はありません。他のシステムでするように
あなた自身の特注のセキュリティコードを書くことができます。


.. Example: :ref:`enabling_authorization_policy`.

例: :ref:`enabling_authorization_policy`


.. Traversal

トラバーサル
~~~~~~~~~~~~

.. :term:`Traversal` is a concept stolen from :term:`Zope`.  It allows you to
.. create a tree of resources, each of which can be addressed by one or more
.. URLs.  Each of those resources can have one or more *views* associated with
.. it. If your data isn't naturally treelike (or you're unwilling to create a
.. treelike representation of your data), you aren't going to find traversal
.. very useful.  However, traversal is absolutely fantastic for sites that need
.. to be arbitrarily extensible: it's a lot easier to add a node to a tree than
.. it is to shoehorn a route into an ordered list of other routes, or to create
.. another entire instance of an application to service a department and glue
.. code to allow disparate apps to share data.  It's a great fit for sites that
.. naturally lend themselves to changing departmental hierarchies, such as
.. content management systems and document management systems.  Traversal also lends itself well to
.. systems that require very granular security ("Bob can edit *this* document"
.. as opposed to "Bob can edit documents").

:term:`Traversal` は :term:`Zope` から盗まれた概念です。それは各々の
リソースが 複数の URL から参照されるリソースツリーを作成することを可能
にします。各々のリソースはそれに関連付けられた複数の *ビュー* を持つこと
ができます。データが自然にはツリーのようでない場合 (あるいは、データの
ツリー状の表現を作成したくない場合)、トラバーサルは非常に有用であるとは感じ
られないかもしれません。しかし、任意に拡張可能である必要のあるサイトで
は、トラバーサルは間違いなく fantastic です: ツリーにノードを加えるのは、
ルートを他のルートの順序付きリストに押し込めたり、一部門をサービスする
のにアプリケーション全体の別のインスタンスを作成して異種のアプリケーション
がデータを共有するためのグルーコードを作成することよりも、ずっと簡単です。
それは、コンテンツ管理システムやドキュメント管理システムのような、
本質的に部門の階層の変更が発生しがちなサイトには非常に適しています。
トラバーサルは、さらに非常に細かい粒度のセキュリティ(「ボブはドキュメント
を編集することができる」に対して「ボブは *この* ドキュメントを編集する
ことができる」)を要求するシステムにも向いています


.. Example: :ref:`hello_traversal_chapter` and :ref:`much_ado_about_traversal_chapter`.

例: :ref:`hello_traversal_chapter` と :ref:`much_ado_about_traversal_chapter`


Tweens
~~~~~~

.. Pyramid has a sort of internal WSGI-middleware-ish pipeline that can be
.. hooked by arbitrary add-ons named "tweens".  The debug toolbar is a "tween",
.. and the ``pyramid_tm`` transaction manager is also.  Tweens are more useful
.. than WSGI middleware in some circumstances because they run in the context of
.. Pyramid itself, meaning you have access to templates and other renderers, a
.. "real" request object, and other niceties.

Pyramid には、 "tween" と呼ばれる任意のアドオンによってフックすること
ができる一種の内部 WSGI ミドルウェア的なパイプラインがあります。 デバッグ
ツールバーは "tween" です。 ``pyramid_tm`` トランザクション管理もそうです。
tween は Pyramid 自身のコンテキスト内で実行されるので、いくつかの状況
で WSGI ミドルウェアより有用です。これは、テンプレートと他のレンダラー、
「本物の」 request オブジェクトや、その他の細々したものにアクセスできる
ことを意味しています。


.. Example: :ref:`registering_tweens`.

例: :ref:`registering_tweens`


.. View response adapters

ビューレスポンスアダプタ
~~~~~~~~~~~~~~~~~~~~~~~~

.. A lot is made of the aesthetics of what *kinds* of objects you're allowed to
.. return from view callables in various frameworks.  In a previous section in
.. this document we showed you that, if you use a :term:`renderer`, you can
.. usually return a dictionary from a view callable instead of a full-on
.. :term:`Response` object.  But some frameworks allow you to return strings or
.. tuples from view callables.  When frameworks allow for this, code looks
.. slightly prettier, because fewer imports need to be done, and there is less
.. code.  For example, compare this:

様々なフレームワークで、ビュー callable からどんな *種類* のオブジェクト
を返すことが認められるかに関する哲学から多くのことが作られています。
このドキュメントの前のセクションでは、 :term:`renderer` を使用すれば
通常はビュー callable から完全な :term:`Response` オブジェクトの代わりに
辞書を返すことができることを示しました。しかし、いくつかのフレームワーク
では、ビュー callable から文字列またはタプルを返すことができます。
フレームワークがこれを許している場合、必要なインポートがより少なくなるので
コードはわずかにきれいに見えます。また、コード量は少なくなります。
例えば、これと:


.. code-block:: python
   :linenos:

   def aview(request):
       return "Hello world!"


.. To this:

これを比較してください:


.. code-block:: python
   :linenos:

   from pyramid.response import Response

   def aview(request):
       return Response("Hello world!")


.. The former is "prettier", right?

前者の方が「きれい」です。そうですね。


.. Out of the box, if you define the former view callable (the one that simply
.. returns a string) in Pyramid, when it is executed, Pyramid will raise an
.. exception.  This is because "explicit is better than implicit", in most
.. cases, and by default, Pyramid wants you to return a :term:`Response` object
.. from a view callable.  This is because there's usually a heck of a lot more
.. to a response object than just its body.  But if you're the kind of person
.. who values such aesthetics, we have an easy way to allow for this sort of
.. thing:

初期状態では、 Pyramid で前者のビュー callable (単に文字列を返すもの)
を定義すると実行時に例外が発生します。これは、「暗黙的より明示的な方が
良い」という理由です。ほとんどの場合、そしてデフォルトでは、 Pyramid は
ビュー callable から :term:`Response` オブジェクトが返されることを期待
します。これは、レスポンスオブジェクトには通常単なるボディよりもずっと
多くの情報が含まれているからです。しかし、あなたがそのような美学を評価する
タイプの人なら、この種のことを可能にする簡単な方法があります:


.. code-block:: python
   :linenos:

   from pyramid.config import Configurator
   from pyramid.response import Response

   def string_response_adapter(s):
       response = Response(s)
       response.content_type = 'text/html'
       return response

   if __name__ == '__main__':
       config = Configurator()
       config.add_response_adapter(string_response_adapter, basestring)


.. Do that once in your Pyramid application at startup.  Now you can return
.. strings from any of your view callables, e.g.:

あなたの Pyramid アプリケーションでスタートアップ時にそれを一度してくだ
さい。すると、任意のビュー callable から文字列を返すことができます。
例えば:


.. code-block:: python
   :linenos:

   def helloview(request):
       return "Hello world!"

   def goodbyeview(request):
       return "Goodbye world!"


.. Oh noes!  What if you want to indicate a custom content type?  And a custom
.. status code?  No fear:

おっと! もしカスタムなコンテントタイプを指定したかったら
どうしますか? カスタムなステータスコードは? 心配ありません。


.. code-block:: python
   :linenos:

   from pyramid.config import Configurator

   def tuple_response_adapter(val):
       status_int, content_type, body = val
       response = Response(body)
       response.content_type = content_type
       response.status_int = status_int
       return response

   def string_response_adapter(body):
       response = Response(body)
       response.content_type = 'text/html'
       response.status_int = 200
       return response

   if __name__ == '__main__':
       config = Configurator()
       config.add_response_adapter(string_response_adapter, basestring)
       config.add_response_adapter(tuple_response_adapter, tuple)


.. Once this is done, both of these view callables will work:

一旦これが終われば、これらのビュー callable は両方とも動くでしょう:


.. code-block:: python
   :linenos:

   def aview(request):
       return "Hello world!"

   def anotherview(request):
       return (403, 'text/plain', "Forbidden")


.. Pyramid defaults to explicit behavior, because it's the most generally
.. useful, but provides hooks that allow you to adapt the framework to localized
.. aesthetic desires.

それが最も一般に有用なので、Pyramid はデフォルトでは明示的に振る舞いますが、
Pyramid はフレームワークを局所的な審美的要望に適応させるためのフックを提供します。


.. See also :ref:`using_iresponse`.

:ref:`using_iresponse` も参照してください。


.. "Global" response object

「グローバル」レスポンスオブジェクト
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. "Constructing these response objects in my view callables is such a chore!
.. And I'm way too lazy to register a response adapter, as per the prior
.. section," you say.  Fine.  Be that way:

「ビュー callable でレスポンスオブジェクトを構築するなんて雑用はたくさん!
それに、前のセクションのようにレスポンスアダプタを登録するのも面倒」
とあなたは言います。よろしい。そのようにしてください:


.. code-block:: python
   :linenos:

   def aview(request):
       response = request.response
       response.body = 'Hello world!'
       response.content_type = 'text/plain'
       return response


.. See also :ref:`request_response_attr`.

:ref:`request_response_attr` も参照してください。


.. Automating repetitive configuration

反復的設定の自動化
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Does Pyramid's configurator allow you to do something, but you're a little
.. adventurous and just want it a little less verbose?  Or you'd like to offer
.. up some handy configuration feature to other Pyramid users without requiring
.. that we change Pyramid?  You can extend Pyramid's :term:`Configurator` with
.. your own directives.  For example, let's say you find yourself calling
.. :meth:`pyramid.config.Configurator.add_view` repetitively.  Usually you can
.. take the boring away by using existing shortcuts, but let's say that this is
.. a case such a way that no existing shortcut works to take the boring away:

Pyramid の configurator だけでできることがあったとして、少し挑戦的に、
もっと冗長でないことを望みますか?  あるいは、ある手軽な設定機能を Pyramid を
変更することを要求せずに他の Pyramid ユーザに届けたいですか?  Pyramid
:term:`Configurator` を拡張して自分のディレクティブを追加することができます。
例えば、 :meth:`pyramid.config.Configurator.add_view` を
繰り返し呼んでいることに気づいたとします。通常、既存のショートカットを
使用することにより退屈な作業を取り除くことができます。しかし、これが
退屈な作業を取り除くために既存のショートカットが働かないような場合で
あるとしましょう:


.. code-block:: python
   :linenos:

   from pyramid.config import Configurator

   config = Configurator()
   config.add_route('xhr_route', '/xhr/{id}')
   config.add_view('my.package.GET_view', route_name='xhr_route',
                   xhr=True,  permission='view', request_method='GET')
   config.add_view('my.package.POST_view', route_name='xhr_route',
                   xhr=True, permission='view', request_method='POST')
   config.add_view('my.package.HEAD_view', route_name='xhr_route',
                   xhr=True, permission='view', request_method='HEAD')


.. Pretty tedious right?  You can add a directive to the Pyramid configurator to
.. automate some of the tedium away:

かなり退屈ですよね? この単調な作業のうちのいくらかを自動化するために
Pyramid configurator にディレクティブを加えることができます:



.. code-block:: python
   :linenos:

   from pyramid.config import Configurator

   def add_protected_xhr_views(config, module):
       module = config.maybe_dotted(module)
       for method in ('GET', 'POST', 'HEAD'):
           view = getattr(module, 'xhr_%s_view' % method, None)
           if view is not None:
               config.add_view(view, route_name='xhr_route', xhr=True, 
                              permission='view', request_method=method)

   config = Configurator()
   config.add_directive('add_protected_xhr_views', add_protected_xhr_views)


.. Once that's done, you can call the directive you've just added as a method of
.. the Configurator object:

それが一旦終われば、たった今追加したディレクティブをConfigurator
オブジェクトのメソッドとして呼ぶことができます:


.. code-block:: python
   :linenos:

   config.add_route('xhr_route', '/xhr/{id}')
   config.add_protected_xhr_views('my.package')


.. Your previously repetitive configuration lines have now morphed into one line.

以前の反復的な設定行は、今や1行に変形しました。


.. You can share your configuration code with others this way too by packaging
.. it up and calling :meth:`~pyramid.config.Configurator.add_directive` from
.. within a function called when another user uses the
.. :meth:`~pyramid.config.Configurator.include` method against your code.

設定コードをパッケージにして、他のユーザがあなたのコードに対する
:meth:`~pyramid.config.Configurator.include` メソッドを使用する時に
呼ばれる関数の内部で :meth:`~pyramid.config.Configurator.add_directive` を
呼ぶことにより、この方法で設定コードを他人と共有することもできます。


.. See also :ref:`add_directive`.

:ref:`add_directive` も参照してください。


.. Testing

テスト
~~~~~~~

.. Every release of Pyramid has 100% statement coverage via unit and integration
.. tests, as measured by the ``coverage`` tool available on PyPI.  It also has
.. greater than 95% decision/condition coverage as measured by the
.. ``instrumental`` tool available on PyPI.  It is automatically tested by the
.. Jenkins tool on Python 2.6, Python 2.7, Python 3.2 and PyPy after each commit
.. to its GitHub repository.  Official Pyramid add-ons are held to a similar
.. testing standard.  We still find bugs in Pyramid and its official add-ons,
.. but we've noticed we find a lot more of them while working on other projects
.. that don't have a good testing regime.

Pyramid のすべてのリリースは、ユニットテストと統合テストによって 100%
のステートメントカバレッジを持ちます。これは PyPI で入手可能な
``coverage`` ツールによって測定されています。さらに、 PyPI で利用可能な
``instrumental`` ツールによる測定で 95% 以上の decision/condition
カバレッジを持ちます。それは GitHub リポジトリのそれぞれのコミットの後で
Jenkins ツールによって Python 2.6, Python 2.7, Python 3.2, PyPy 上で
自動的にテストされます。公式の Pyramid アドオンは同様のテスト基準に
保たれます。 Pyramid およびその公式アドオンには今もバグが見つかりますが、
私たちは良いテストを行う regime (体制) を持たない他のプロジェクトで働いている間に、
バグがより多く見つかることに気づきました。


.. Example: http://jenkins.pylonsproject.org/

例: http://jenkins.pylonsproject.org/


.. Support

サポート
~~~~~~~~

.. It's our goal that no Pyramid question go unanswered.  Whether you ask a
.. question on IRC, on the Pylons-discuss maillist, or on StackOverflow, you're
.. likely to get a reasonably prompt response.  We don't tolerate "support
.. trolls" or other people who seem to get their rocks off by berating fellow
.. users in our various offical support channels.  We try to keep it well-lit
.. and new-user-friendly.

私たちのゴールは、答えられない Pyramid に関する質問がないことです。
IRC や Pylons-discuss メーリングリスト、あるいは StackOverflow のいずれか
で質問をしても、合理的に迅速なレスポンスが得られるでしょう。私たちは、
"support trolls" あるいは様々な公式のサポートチャンネルにおいて仲間の
ユーザを非難することに喜びを見い出すような人々を許容しません。
私たちは、そこを well-lit (明るく照らされた)、新規ユーザーに優しい場所に保とうとしています。


.. Example: Visit irc\://freenode.net#pyramid (the ``#pyramid`` channel on
.. irc.freenode.net in an IRC client) or the pylons-discuss maillist at
.. http://groups.google.com/group/pylons-discuss/ .

例: irc\://freenode.net#pyramid (IRC クライアント中では
irc.freenode.net の ``#pyramid`` チャンネル) や pylons-discuss メーリング
リスト http://groups.google.com/group/pylons-discuss/ を訪ねてください。


.. Documentation

ドキュメント
~~~~~~~~~~~~~

.. It's a constant struggle, but we try to maintain a balance between
.. completeness and new-user-friendliness in the official narrative Pyramid
.. documentation (concrete suggestions for improvement are always appreciated,
.. by the way).  We also maintain a "cookbook" of recipes, which are usually
.. demonstrations of common integration scenarios, too specific to add to the
.. official narrative docs.  In any case, the Pyramid documentation is
.. comprehensive.

それは絶え間ない努力です。しかし、私たちは公式の narrative Pyramid
ドキュメンテーションの中で完全性と新規ユーザーフレンドリーの間のバランスを
維持しようとします (それはそれとして、具体的な改善提案は常に感謝されます)。
さらに、私たちはレシピの「クックブック」も維持します。それらは通常、よく
ある統合シナリオのデモンストレーションで、公式 narrative ドキュメントに
追加するには特殊すぎるものです。どんな場合も、 Pyramid ドキュメンテーション
は包括的です。


.. Example: The rest of this documentation and the cookbook at
.. http://docs.pylonsproject.org/projects/pyramid_cookbook/dev/ .

例: このドキュメントの残りとクックブック
http://docs.pylonsproject.org/projects/pyramid_cookbook/dev/


.. index::
   single: Pylons Project


.. What Is The Pylons Project?

Pylons プロジェクトとは?
---------------------------

.. :app:`Pyramid` is a member of the collection of software published under the
.. Pylons Project.  Pylons software is written by a loose-knit community of
.. contributors.  The `Pylons Project website <http://pylonsproject.org>`_
.. includes details about how :app:`Pyramid` relates to the Pylons Project.

:app:`Pyramid` は Pylons プロジェクトの下で公開されたソフトウェアコレク
ションのメンバーです。Pylons ソフトウェアは、貢献者が緩く結合したコミュ
ニティーによって書かれています。 `Pylons プロジェクトのウェブサイト
<http://pylonsproject.org>`_ には、 :app:`Pyramid` と Pylons プロジェクト
の関係についての詳細があります。


.. index::
   single: pyramid and other frameworks
   single: Zope
   single: Pylons
   single: Django
   single: MVC


.. :app:`Pyramid` and Other Web Frameworks

:app:`Pyramid` と他のウェブフレームワーク
------------------------------------------

.. The first release of Pyramid's predecessor (named :mod:`repoze.bfg`) was made
.. in July of 2008.  At the end of 2010, we changed the name of
.. :mod:`repoze.bfg` to :app:`Pyramid`.  It was merged into the Pylons project
.. as :app:`Pyramid` in November of that year.

Pyramid の前身 (:mod:`repoze.bfg` という名前) の最初のリリースは 2008
年の 7 月に行われました。 2010 年の終わりに、私たちは :mod:`repoze.bfg`
の名前を :app:`Pyramid` に変更しました。そしてその年の 11 月に
:app:`Pyramid` として Pylons プロジェクトへと合併されました。


.. :app:`Pyramid` was inspired by :term:`Zope`, :term:`Pylons` (version
.. 1.0) and :term:`Django`.  As a result, :app:`Pyramid` borrows several
.. concepts and features from each, combining them into a unique web
.. framework.

:app:`Pyramid` は、 :term:`Zope`, :term:`Pylons` (バージョン 1.0),
:term:`Django` にインスパイアされました。その結果、 :app:`Pyramid` は
各々からいくつかの概念や機能を借りると同時にそれらを組み合わせて
ユニークなウェブフレームワークになっています。


.. Many features of :app:`Pyramid` trace their origins back to :term:`Zope`.
.. Like Zope applications, :app:`Pyramid` applications can be easily extended:
.. if you obey certain constraints, the application you produce can be reused,
.. modified, re-integrated, or extended by third-party developers without
.. forking the original application.  The concepts of :term:`traversal` and
.. declarative security in :app:`Pyramid` were pioneered first in Zope.

:app:`Pyramid` の多くの特徴は、その起源を :term:`Zope` までさかのぼるこ
とができます。 Zope アプリケーションと同様、 :app:`Pyramid` アプリケー
ションは容易に拡張することができます: ある制約に従って作れば、あなたの
アプリケーションをサードパーティの開発者が元のアプリケーションをフォー
クすることなく、再利用したり、修正したり、再統合したり、拡張したりする
ことができます。 :app:`Pyramid` の中の :term:`traversal` および宣言的な
セキュリティの概念は、Zopeの中で最初に開拓されました。


.. The :app:`Pyramid` concept of :term:`URL dispatch` is inspired by the
.. :term:`Routes` system used by :term:`Pylons` version 1.0.  Like Pylons
.. version 1.0, :app:`Pyramid` is mostly policy-free.  It makes no
.. assertions about which database you should use, and its built-in
.. templating facilities are included only for convenience.  In essence,
.. it only supplies a mechanism to map URLs to :term:`view` code, along
.. with a set of conventions for calling those views.  You are free to
.. use third-party components that fit your needs in your applications.

:app:`Pyramid` の :term:`URL dispatch` 概念は、 :term:`Pylons` バージョン
1.0 で使用される :term:`Routes` システムによってインスパイアされました。
Pylons バージョン 1.0 のように、 :app:`Pyramid` はほぼポリシーフリーです。
それは、どのデータベースを使用すべきといった主張をせず、内蔵のテンプレート
機能は利便性のためにのみ含まれています。本質的には、それは単に URL を
:term:`view` コードにマッピングするためのメカニズムと、それらのビューを
呼び出すための規約一式を提供します。アプリケーションの中であなたのニーズに
合ったサードパーティのコンポーネントを自由に使用することができます。


.. The concept of :term:`view` is used by :app:`Pyramid` mostly as it would be
.. by Django.  :app:`Pyramid` has a documentation culture more like Django's
.. than like Zope's.

:app:`Pyramid` によって使用される :term:`view` の概念は、ほとんど Django
のそれと同じです。 :app:`Pyramid` には Zope よりもむしろ Django のような
ドキュメンテーション文化があります。


.. Like :term:`Pylons` version 1.0, but unlike :term:`Zope`, a :app:`Pyramid`
.. application developer may use completely imperative code to perform common
.. framework configuration tasks such as adding a view or a route.  In Zope,
.. :term:`ZCML` is typically required for similar purposes.  In :term:`Grok`, a
.. Zope-based web framework, :term:`decorator` objects and class-level
.. declarations are used for this purpose.  Out of the box, Pyramid supports
.. imperative and decorator-based configuration; :term:`ZCML` may be used via an
.. add-on package named ``pyramid_zcml``.

:term:`Pylons` バージョン 1.0 のように、しかし :term:`Zope` とは違い、
:app:`Pyramid` アプリケーション開発者は、ビューやルートを追加するような
よくあるフレームワーク設定タスクを行なうために完全に命令的なコードを
使用することができます。Zope では、同様の目的のために典型的に
:term:`ZCML` が必要です。 :term:`Grok` という Zope に基づくウェブフレーム
ワークの中では、この目的のために :term:`decorator` オブジェクトとクラス
レベル宣言が使用されます。初期状態で、 Pyramid は命令的な設定とデコレータ
に基づいた設定をサポートします; :term:`ZCML` は ``pyramid_zcml`` という
名前のアドオンパッケージによって使用することができます。


.. Also unlike :term:`Zope` and unlike other "full-stack" frameworks such
.. as :term:`Django`, :app:`Pyramid` makes no assumptions about which
.. persistence mechanisms you should use to build an application.  Zope
.. applications are typically reliant on :term:`ZODB`; :app:`Pyramid`
.. allows you to build :term:`ZODB` applications, but it has no reliance
.. on the ZODB software.  Likewise, :term:`Django` tends to assume that
.. you want to store your application's data in a relational database.
.. :app:`Pyramid` makes no such assumption; it allows you to use a
.. relational database but doesn't encourage or discourage the decision.

:term:`Zope` とは異なり、また :term:`Django` のような他の「フルスタック」
のフレームワークと異なり、 :app:`Pyramid` は、アプリケーションを構築す
るためにどの永続化メカニズムを使用すべきかに関する前提を置きません。
Zope アプリケーションは、典型的に :term:`ZODB` に依存しています;
:app:`Pyramid` を使って :term:`ZODB` アプリケーションを構築することは
できますが、 ZODB ソフトウェアに対する依存はありません。
同様に、 :term:`Django` はアプリケーションのデータをリレーショナル
データベースに格納すると仮定する傾向にありますが、 :app:`Pyramid` は
そのような仮定をしません; それは、あなたがリレーショナルデータベースを
使用することを可能にしますが、決定に賛成も反対もしません。


.. Other Python web frameworks advertise themselves as members of a class
.. of web frameworks named `model-view-controller
.. <http://en.wikipedia.org/wiki/Model–view–controller>`_ frameworks.
.. Insofar as this term has been claimed to represent a class of web
.. frameworks, :app:`Pyramid` also generally fits into this class.

他の Python ウェブフレームワークは、 `model-view-controller
<http://en.wikipedia.org/wiki/Model–view–controller>`_ フレームワークと
いう名前のウェブフレームワークのクラスのメンバーとして自己宣伝しています。
この用語がウェブフレームワークのクラスを表わすために主張される限りにおいて
:app:`Pyramid` も一般にこのクラスに入ります。


.. .. sidebar:: You Say :app:`Pyramid` is MVC, But Where's The Controller?

..    The :app:`Pyramid` authors believe that the MVC pattern just doesn't
..    really fit the web very well. In a :app:`Pyramid` application, there is a
..    resource tree, which represents the site structure, and views, which tend
..    to present the data stored in the resource tree and a user-defined "domain
..    model".  However, no facility provided *by the framework* actually
..    necessarily maps to the concept of a "controller" or "model".  So if you
..    had to give it some acronym, I guess you'd say :app:`Pyramid` is actually
..    an "RV" framework rather than an "MVC" framework.  "MVC", however, is
..    close enough as a general classification moniker for purposes of
..    comparison with other web frameworks.


.. sidebar:: You Say :app:`Pyramid` is MVC, But Where's The Controller?

   :app:`Pyramid` が MVC だと言うけれど、コントローラーはどこ?

   :app:`Pyramid` の作者は、 MVC パターンが実際にはウェブにあまりぴった
   りとフィットしないと信じています。 :app:`Pyramid` アプリケーションでは、
   サイト構造を表すリソースツリーと、リソースツリーとユーザ定義の
   「ドメインモデル」に保存されたデータを表示する傾向にあるビューがあります。
   しかしながら、 *フレームワークによって* 提供される機能は、必ずしも
   実際に「コントローラ」あるいは「モデル」の概念にマッピングされません。
   したがって、それに頭字語を与えなければならないとしたら、私は
   :app:`Pyramid` が実際には「MVC」フレームワークというよりむしろ「RV」
   フレームワークであると言うべきだろうと思います。しかし、「MVC」は、
   他のウェブフレームワークとの比較の目的のための一般的な分類名としては
   十分に近いです。
