.. Adding Authorization

.. _wiki2_adding_authorization:

====================
認証を追加する
====================

.. :app:`Pyramid` provides facilities for :term:`authentication` and
.. :term:`authorization`.  We'll make use of both features to provide security
.. to our application.  Our application currently allows anyone with access to
.. the server to view, edit, and add pages to our wiki.  We'll change that
.. to allow only people who are members of a *group* named ``group:editors``
.. to add and edit wiki pages but we'll continue allowing
.. anyone with access to the server to view pages.

:app:`Pyramid` は :term:`authentication` と :term:`authorization` の
ための機能を提供しています。アプリケーションにセキュリティを追加する
ためにこれら両方の機能を利用します。私たちのアプリケーションは現在、
サーバーにアクセスできる誰もが wiki ページを見たり、編集したり、
追加したりすることが可能です。 wiki ページの追加や編集を ``group:editors``
という名前の *グループ* のメンバーである限られた人々だけに許可するように
アプリケーションを変更してみましょう。しかし、依然としてサーバーに
アクセスできる誰でもページを見ることは可能です。


.. We will also add a login page and a logout link on all the
.. pages.  The login page will be shown when a user is denied
.. access to any of the views that require a permission, instead of
.. a default "403 Forbidden" page.

さらに、ログインページとすべてのページにログアウトリンクを追加します。
ログインページは、ユーザがパーミッションを必要とする任意のビューへのアクセスを
拒否された場合に、デフォルトの "403 Forbidden" ページの代わりに表示されます。


.. We will implement the access control with the following steps:

次のステップでアクセス制御を実装していきます。


.. * Add users and groups (``security.py``, a new module).
.. * Add an :term:`ACL` (``models.py`` and
..   ``__init__.py``).
.. * Add an :term:`authentication policy` and an :term:`authorization policy`
..   (``__init__.py``).
.. * Add :term:`permission` declarations to the ``edit_page`` and ``add_page``
..   views (``views.py``).

* ユーザとグループを追加 (``security.py`` という新しいモジュール)
* :term:`ACL` を追加 (``models.py`` と ``__init__.py``)
* :term:`authentication policy` と :term:`authorization policy` を追加
  (``__init__.py``)
* ``edit_page`` および ``add_page`` ビューに :term:`permission` 宣言を
  追加 (``views.py``)


.. Then we will add the login and logout feature:

次にログインとログアウトの機能を追加します:


.. * Add routes for /login and /logout (``__init__.py``).
.. * Add ``login`` and ``logout`` views (``views.py``).
.. * Add a login template (``login.pt``).
.. * Make the existing views return a ``logged_in`` flag to the renderer (``views.py``).
.. * Add a "Logout" link to be shown when logged in and viewing or editing a page
..   (``view.pt``, ``edit.pt``).

* /login と /logout に対する route を追加 (``__init__.py``)
* ``login`` および ``logout`` ビューを追加 (``views.py``)
* ログインテンプレートを追加 (``login.pt``)
* 既存のビューがレンダラーに ``logged_in`` フラグを返すように修正 (``views.py``)
* "Logout" リンクを追加して、ログイン中にページを閲覧または編集しているときに
  表示されるようにする (``view.pt``, ``edit.pt``)


.. The source code for this tutorial stage can be browsed at
.. `http://github.com/Pylons/pyramid/tree/1.3-branch/docs/tutorials/wiki2/src/authorization/
.. <http://github.com/Pylons/pyramid/tree/1.3-branch/docs/tutorials/wiki2/src/authorization/>`_.

このチュートリアルステージのソースコードを以下の場所で閲覧することができます。
`http://github.com/Pylons/pyramid/tree/1.3-branch/docs/tutorials/wiki2/src/authorization/
<http://github.com/Pylons/pyramid/tree/1.3-branch/docs/tutorials/wiki2/src/authorization/>`_.


.. Access Control

アクセス制御
--------------

.. Add users and groups

ユーザとグループの追加
~~~~~~~~~~~~~~~~~~~~~~

.. Create a new ``tutorial/tutorial/security.py`` module with the
.. following content:

新しい ``tutorial/tutorial/security.py`` モジュールを以下の内容で作成します:


.. literalinclude:: src/authorization/tutorial/security.py
   :linenos:
   :language: python


.. The ``groupfinder`` function accepts a userid and a request and
.. returns one of these values:

``groupfinder`` 関数は、 userid と request を受け取って、以下のどちらかを
返します:


.. - If the userid exists in the system, it will return a
..   sequence of group identifiers (or an empty sequence if the user
..   isn't a member of any groups).
.. - If the userid *does not* exist in the system, it will
..   return ``None``.

- userid がシステムに存在すれば、グループ識別子のシーケンス
  (ユーザがどのグループのメンバーでもなければ空のシーケンス) を返します。
- userid がシステムに存在 *しなければ* 、 ``None`` を返します。


.. For example, ``groupfinder('editor', request )`` returns ['group:editor'],
.. ``groupfinder('viewer', request)`` returns [], and ``groupfinder('admin', request)``
.. returns ``None``.  We will use ``groupfinder()`` as an :term:`authentication policy`
.. "callback" that will provide the :term:`principal` or principals
.. for a user.

例えば、 ``groupfinder('editor', request )`` は ['group:editor'] を返し、
``groupfinder('viewer', request)`` は [] を、
``groupfinder('admin', request)`` は ``None`` を返します。
あとで、ユーザに :term:`principal` (複数可) を提供する
:term:`authentication policy` "コールバック" として ``groupfinder()``
を使用します。


.. In a production system, user and group
.. data will most often come from a database, but here we use "dummy"
.. data to represent user and groups sources.

プロダクションシステムでは、ユーザとグループデータは、ほとんどの場合
データベースから取得されますが、ここではユーザとグループのソースを表わすために
「ダミーの」データを使用します。


.. Add an ACL

ACL の追加
~~~~~~~~~~

.. Open ``tutorial/tutorial/models.py`` and add the following import
.. statement at the head:

``tutorial/tutorial/models.py`` を開いて先頭に以下のインポート文を
追加してください:


.. literalinclude:: src/authorization/tutorial/models.py
   :lines: 1-4
   :linenos:
   :language: python


.. Add the following class definition:

以下のクラス定義を追加してください:


.. literalinclude:: src/authorization/tutorial/models.py
   :lines: 36-40
   :linenos:
   :language: python


.. We import :data:`~pyramid.security.Allow`, an action that
.. means that permission is allowed:, and
.. :data:`~pyramid.security.Everyone`, a special :term:`principal`
.. that is associated to all requests.  Both are used in the
.. :term:`ACE` entries that make up the ACL.

:data:`~pyramid.security.Allow` (パーミッションが許可されることを意味す
るアクション) と :data:`~pyramid.security.Everyone` (すべてのリクエスト
に関連付けられる特別な :term:`principal`) をインポートします。両者は ACL
を構成するために :term:`ACE` エントリの中で使用されます。


.. The ACL is a list that needs to be named `__acl__` and be an
.. attribute of a class.  We define an :term:`ACL` with two
.. :term:`ACE` entries: the first entry allows any user the `view`
.. permission.  The second entry allows the ``group:editors``
.. principal  the `edit` permission.

ACL はリストで、 `__acl__` という名前のクラス属性である必要があります。
ここでは2つの :term:`ACE` エントリを持つ :term:`ACL` を定義しています:
1番目のエントリは、あらゆるユーザに `view` パーミッションを与えます。
2番目のエントリは、 ``group:editors`` principal に `edit` パーミッション
を与えます。


.. The ``RootFactory`` class that contains the ACL is a :term:`root factory`.
.. We need to associate it to our :app:`Pyramid` application, so the ACL is
.. provided to each view in the :term:`context` of the request, as
.. the ``context`` attribute.

ACL を含む ``RootFactory`` クラスは :term:`root factory` です。
それを :app:`Pyramid` アプリケーションに関連付ける必要があります。
それにより ACL が各ビューに対してリクエストの :term:`context` で
(``context`` 属性として) 提供されます。


.. Open ``tutorial/tutorial/__init__.py`` and add a ``root_factory``
.. parameter to our :term:`Configurator` constructor, that points to
.. the class we created above:

``tutorial/tutorial/__init__.py`` を開いて、 :term:`Configurator`
コンストラクタに ``root_factory`` パラメータを追加してください。
それは上で作成したクラスを指します:


.. literalinclude:: src/authorization/tutorial/__init__.py
   :lines: 24-25
   :linenos:
   :emphasize-lines: 2
   :language: python


.. (Only the highlighted line needs to be added.)

(ハイライトされた行は変更が必要な箇所です)


.. We are now providing the ACL to the application.  See
.. :ref:`assigning_acls` for more information about what an
.. :term:`ACL` represents.

これでアプリケーションに ACL を渡せるようになりました。 :term:`ACL` が
何を表わすかについての詳細は :ref:`assigning_acls` を参照してください。


.. .. note::

..     Although we don't use the functionality here, the ``factory`` used
..     to create route contexts may differ per-route as opposed to globally.  See
..     the ``factory`` argument to
..     :meth:`pyramid.config.Configurator.add_route` for more info.

.. note:

    ここではその機能を使いませんが、ルートコンテキストを生成するために
    使われる ``factory`` は、グローバルにする代わりにルート毎に異なって
    いても構いません。より多くの情報については、
    :meth:`pyramid.config.Configurator.add_route` への ``factory`` 引数
    を参照してください。


.. Add Authentication and Authorization Policies

認証と認可のポリシーを追加する
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Open ``tutorial/__init__.py`` and
.. add these import statements:

``tutorial/__init__.py`` を開いて、これらのインポート文を追加してください:


.. literalinclude:: src/authorization/tutorial/__init__.py
   :lines: 2-3,7
   :linenos:
   :language: python


.. Now add those policies to the configuration:

そうしたら、設定にこれらのポリシーを加えてください:


.. literalinclude:: src/authorization/tutorial/__init__.py
   :lines: 21-27
   :linenos:
   :emphasize-lines: 1-3,6-7
   :language: python


.. (Only the highlighted lines need to be added.)

(ハイライトされた行は変更が必要な箇所です)


.. We are enabling an ``AuthTktAuthenticationPolicy``, it is based in an
.. auth ticket that may be included in the request, and an
.. ``ACLAuthorizationPolicy`` that uses an ACL to determine the allow or deny
.. outcome for a view.

``AuthTktAuthenticationPolicy`` を有効にしています。
それは、リクエストに含まれている可能性のある auth チケットと、
ACL を使用してビューに対する許可または禁止の結果を決定する
``ACLAuthorizationPolicy`` に基づきます。


.. Note that the :class:`pyramid.authentication.AuthTktAuthenticationPolicy`
.. constructor accepts two arguments: ``secret`` and ``callback``.  ``secret`` is
.. a string representing an encryption key used by the "authentication ticket"
.. machinery represented by this policy: it is required.  The ``callback`` is the
.. ``groupfinder()`` function that we created before.

:class:`pyramid.authentication.AuthTktAuthenticationPolicy` コンストラクタ
は 2 つの引数を受け付けます: ``secret`` と ``callback`` です。 ``secret`` は、
このポリシーによって表わされる「認証チケット」機構によって使用される
暗号鍵を表わす文字列です: これは必須です。 ``callback`` は、以前作成した
``groupfinder`` 関数です。


.. Add permision declarations

パーミッション宣言を追加する
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Add a ``permission='edit'`` parameter to the ``@view_config``
.. decorator for ``add_page()`` and ``edit_page()``, for example:

``permission='edit'`` パラメータを ``add_page()`` と ``edit_page()`` に対する
``@view_config`` デコレータに追加してください。例えば:


.. code-block:: python
   :linenos:
   :emphasize-lines: 2

   @view_config(route_name='add_page', renderer='templates/edit.pt',
                permission='edit')


.. (Only the highlighted line needs to be added.)

(ハイライトされた行は変更が必要な箇所です)


.. The result is that only users who possess the ``edit``
.. permission at the time of the request may invoke those two views.

その結果は、リクエストの時点で ``edit`` パーミッションを所有するユーザ
だけが、それら2つのビューを起動できるということです。


.. Add a ``permission='view'`` parameter to the ``@view_config``
.. decorator for ``view_wiki()`` and ``view_page()``, like this:

``permission='view'`` パラメータを ``view_wiki()`` と ``view_page()`` に対する
``@view_config`` デコレータに追加してください。例えば:


.. code-block:: python
   :linenos:
   :emphasize-lines: 2

   @view_config(route_name='view_page', renderer='templates/view.pt',
                permission='view')


.. (Only the highlighted line needs to be added.)

(ハイライトされた行は変更が必要な箇所です)


.. This allows anyone to invoke these two views.

これは、誰でもこれら2つのビューを起動できるようにします。


.. We are done with the changes needed to control access.  The
.. changes that follow will add the login and logout feature.

これでアクセスを制御するのに必要とされる変更が終わりました。
続いての変更は、ログインおよびログアウト機能を追加することです。


.. Login, Logout

ログイン、ログアウト
--------------------

.. Add routes for /login and /logout

/login と /logout に対する route を追加する
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Go back to ``tutorial/tutorial/__init__.py`` and add these two
.. routes:

``tutorial/tutorial/__init__.py`` に戻り、これら2つの route を追加してください:


.. literalinclude:: src/authorization/tutorial/__init__.py
   :lines: 30-31
   :linenos:
   :language: python


.. Add Login and Logout Views

Login と Logout ビューを追加する
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. We'll add a ``login`` view which renders a login form and processes
.. the post from the login form, checking credentials.

ログインフォームをレンダリングしたり、ログインフォームから送信された
データを処理して認証情報 (credentials) をチェックしたりする ``login``
ビューを追加します。


.. We'll also add a ``logout`` view callable to our application and
.. provide a link to it.  This view will clear the credentials of the
.. logged in user and redirect back to the front page.

さらに、アプリケーションに ``logout`` ビュー callable を加えて、
それへのリンクを提供します。このビューは、ログインユーザの
資格をクリアして、フロントページにリダイレクトします。


.. Add the following import statements to the
.. head of ``tutorial/tutorial/views.py``:

``tutorial/tutorial/views.py`` の先頭に、以下のインポート文を追加してください:


.. literalinclude:: src/authorization/tutorial/views.py
   :lines: 9-16,18,24-25
   :linenos:
   :emphasize-lines: 3,6-9,11
   :language: python


.. (Only the highlighted lines need to be added.)

(ハイライトされた行は変更が必要な箇所です)


.. :meth:`~pyramid.view.forbidden_view_config` will be used
.. to customize the default 403 Forbidden page.
.. :meth:`~pyramid.security.remember` and
.. :meth:`~pyramid.security.forget` help to create and
.. expire an auth ticket cookie.

:meth:`~pyramid.view.forbidden_view_config` は、デフォルトの 403
Forbidden ページをカスタマイズするために使用されます。
:meth:`~pyramid.security.remember` と :meth:`~pyramid.security.forget` は、
auth チケットのクッキーの作成と破棄をサポートします。


.. Now add the ``login`` and ``logout`` views:

そして ``login`` および ``logout`` ビューを追加します:


.. literalinclude:: src/authorization/tutorial/views.py
   :lines: 91-123
   :linenos:
   :language: python


.. ``login()`` is decorated with two decorators:

``login`` は 2 つのデコレータでデコレートされています:


.. - a ``@view_config`` decorator which associates it with the
..   ``login`` route and makes it visible when we visit ``/login``,
.. - a ``@forbidden_view_config`` decorator which turns it into
..   an :term:`forbidden view`. ``login()`` will be invoked
..   when a users tries to execute a view callable that
..   they are not allowed to.  For example, if a user has not logged in
..   and tries to add or edit a Wiki page, he will be shown the
..   login form before being allowed to continue on.

- ビュー関数を ``login`` ルートに関連付け、 ``/login`` を訪れたときに
  それが表示されるようにする ``@view_config`` 。
- ビュー関数を :term:`exception view` に変換する ``@forbidden_view_config`` 。
  ``login()`` は、ユーザが許可されないビュー callable を実行しようとする
  時に起動されます。例えば、もしユーザがログインせずに Wiki ページを
  追加または編集しようとすれば、その続きが許可される前にログインフォーム
  が表示されます。


.. The order of these two :term:`view configuration` decorators
.. is unimportant.

これら2つの :term:`view configuration` デコレータの順番は重要ではありません。


.. ``logout()`` is decorated with a ``@view_config`` decorator
.. which associates it with the ``logout`` route.  It will be
.. invoked when we visit ``/logout``.

``logout()`` は、 ``logout`` ルートに関連付ける
``@view_config`` デコレータでデコレートされます。これは、
``/logout`` を訪れたときに起動されます。


.. Add the ``login.pt`` Template

``login.pt`` テンプレートを追加する
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Create ``tutorial/tutorial/templates/login.pt`` with the following
.. content:

以下の内容で ``tutorial/tutorial/templates/login.pt`` を作成してください:


.. literalinclude:: src/authorization/tutorial/templates/login.pt
   :language: xml


.. The above template is referred to within the login view we just
.. added to ``views.py``.

上記のテンプレートは、さきほど ``views.py`` に追加した login ビュー内で
参照されます。


.. Return a logged_in flag to the renderer

logged_in フラグをレンダラーに返す
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Add the following line to the import at the head of
.. ``tutorial/tutorial/views.py``:

``tutorial/tutorial/views.py`` の先頭のインポートに以下の行を追加してください:


.. literalinclude:: src/authorization/tutorial/views.py
   :lines: 14-18
   :linenos:
   :emphasize-lines: 4
   :language: python


.. (Only the highlighted line needs to be added.)

(ハイライトされた行は変更が必要な箇所です)


.. Add a  ``logged_in`` parameter to the return value of
.. ``view_page()``, ``edit_page()`` and  ``add_page()``,
.. like this:

``logged_in`` パラメータを以下のように ``view_page()``, ``edit_page()``,
``add_page()`` の戻り値に追加してください:


.. code-block:: python
   :linenos:
   :emphasize-lines: 4

   return dict(page = page,
               content = content,
               edit_url = edit_url,
               logged_in = authenticated_userid(request))


.. (Only the highlighted line needs to be added.)

(ハイライトされた行は変更が必要な箇所です)


.. :meth:`~pyramid.security.authenticated_userid()` will return None
.. if the user is not authenticated, or some user id it the user
.. is authenticated.

:meth:`~pyramid.security.authenticated_userid()` は、もしユーザが認証
されなければ None を返し、ユーザが認証されれば何らかのユーザ id を
返します。


.. Add a "Logout" link when logged in

ログインした時に "Logout" リンクを追加する
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. Open ``tutorial/tutorial/templates/edit.pt`` and
.. ``tutorial/tutorial/templates/view.pt``  and add this within the
.. ``<div id="right" class="app-welcome align-right">`` div:

``tutorial/tutorial/templates/edit.pt`` と
``tutorial/tutorial/templates/view.pt`` を開いて
``<div id="right" class="app-welcome align-right">`` div の内側にこれを
追加してください:


.. code-block:: xml

   <span tal:condition="logged_in">
      <a href="${request.application_url}/logout">Logout</a>
   </span>


.. The attribute ``tal:condition="logged_in"`` will make the element be
.. included when ``logged_in`` is any user id. The link will invoke
.. the logout view.  The above element will not be included if ``logged_in``
.. is ``None``, such as when a user is not authenticated.

``logged_in`` が任意のユーザ id である場合、属性 ``tal:condition="logged_in"``
が要素に含まれるようになります。このリンクはログアウトビューを起動します。
ユーザが認証されていない場合のように ``logged_in`` が ``None`` なら、上記の
要素は含まれません。


.. Seeing Our Changes

変更点を確認する
------------------

.. Our ``tutorial/tutorial/__init__.py`` will look something like this
.. when we're done:

最終的に ``tutorial/tutorial/__init__.py`` はこのようになっているはずです:


.. literalinclude:: src/authorization/tutorial/__init__.py
   :linenos:
   :emphasize-lines: 2-3,7,21-23,25-27,30-31
   :language: python


.. (Only the highlighted lines need to be added.)

(ハイライトされた行は変更が必要な箇所です)


.. Our ``tutorial/tutorial/models.py`` will look something like this
.. when we're done:

最終的に ``tutorial/tutorial/models.py`` はこのようになっているはずです:


.. literalinclude:: src/authorization/tutorial/models.py
   :linenos:
   :emphasize-lines: 1-4,36-40
   :language: python


.. (Only the highlighted lines need to be added.)

(ハイライトされた行は変更が必要な箇所です)


.. Our ``tutorial/tutorial/views.py`` will look something like this
.. when we're done:

最終的に ``tutorial/tutorial/views.py`` はこのようになっているはずです:


.. literalinclude:: src/authorization/tutorial/views.py
   :linenos:
   :emphasize-lines: 11,14-18,25,31,37,58,61,73,76,88,91-117,119-123
   :language: python


.. (Only the highlighted lines need to be added.)

(ハイライトされた行は変更が必要な箇所です)


.. Our ``tutorial/tutorial/templates/edit.pt`` template will look
.. something like this when we're done:

最終的に ``tutorial/tutorial/templates/edit.pt`` テンプレートは
このようになっているはずです:


.. literalinclude:: src/authorization/tutorial/templates/edit.pt
   :linenos:
   :emphasize-lines: 41-43
   :language: xml


.. (Only the highlighted lines need to be added.)

(ハイライトされた行は変更が必要な箇所です)


.. Our ``tutorial/tutorial/templates/view.pt`` template will look
.. something like this when we're done:

最終的に ``tutorial/tutorial/templates/view.pt`` テンプレートは
このようになっているはずです:


.. literalinclude:: src/authorization/tutorial/templates/view.pt
   :linenos:
   :emphasize-lines: 41-43
   :language: xml


.. (Only the highlighted lines need to be added.)

(ハイライトされた行は変更が必要な箇所です)


.. Viewing the Application in a Browser

ブラウザでアプリケーションを表示する
------------------------------------

.. We can finally examine our application in a browser (See
.. :ref:`wiki2-start-the-application`).  Launch a browser and visit
.. each of the following URLs, check that the result is as expected:

最終的に、ブラウザの中でアプリケーションを実行することができます
(:ref:`wiki2-start-the-application` 参照)。
ブラウザを起動して以下の各 URL にアクセスして、結果が期待通りであることを
確認してください:


.. - ``http://localhost:6543/`` invokes the
..   ``view_wiki`` view.  This always redirects to the ``view_page`` view
..   of the FrontPage page object.  It is executable by any user.

- ``http://localhost:6543/`` は ``view_wiki`` ビューを起動します。
  これは常に FrontPage page オブジェクトの ``view_page`` ビューに
  リダイレクトします。このビューは任意のユーザによって実行可能です。


.. - ``http://localhost:6543/FrontPage`` invokes
..   the ``view_page`` view of the FrontPage page object.

- ``http://localhost:6543/FrontPage`` は FrontPage page オブジェクトの
  ``view_page`` ビューを起動します。


.. - ``http://localhost:6543/FrontPage/edit_page``
..   invokes the edit view for the FrontPage object.  It is executable by
..   only the ``editor`` user.  If a different user (or the anonymous
..   user) invokes it, a login form will be displayed.  Supplying the
..   credentials with the username ``editor``, password ``editor`` will
..   display the edit page form.

- ``http://localhost:6543/FrontPage/edit_page`` は FrontPage オブジェクト
  に対応する edit ビューを起動します。それは ``editor`` ユーザだけが
  実行可能です。異なるユーザ (あるいは匿名ユーザ) が起動すると、ログイン
  フォームが表示されます。認証情報としてユーザー名 ``editor`` 、パスワード
  ``editor`` を入力すると編集ページフォームが表示されるでしょう。


.. - ``http://localhost:6543/add_page/SomePageName``
..   invokes the add view for a page.  It is executable by only
..   the ``editor`` user.  If a different user (or the anonymous user)
..   invokes it, a login form will be displayed.  Supplying the
..   credentials with the username ``editor``, password ``editor`` will
..   display the edit page form.

- ``http://localhost:6543/add_page/SomePageName`` は、ページに対する
  add ビューを起動します。それは ``editor`` ユーザだけが実行可能です。
  異なるユーザ (あるいは匿名ユーザ) が起動すると、ログインフォームが
  表示されます。認証情報としてユーザー名 ``editor`` 、パスワード
  ``editor`` を入力すると編集ページフォームが表示されるでしょう。


.. - After logging in (as a result of hitting an edit or add page
..   and submitting the login form with the ``editor``
..   credentials), we'll see a Logout link in the upper right hand
..   corner.  When we click it, we're logged out, and redirected
..   back to the front page.

- (edit または add ページにアクセスしてログインフォームに ``editor``
  認証を送信した結果として) ログイン後に、右上に Logout リンクが表示
  されているでしょう。それをクリックするとログアウトして、
  フロントページにリダイレクトされます。
