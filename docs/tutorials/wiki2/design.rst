.. Design

==========
設計
==========

.. Following is a quick overview of the design of our wiki application, to help
.. us understand the changes that we will be making as we work through the
.. tutorial.

下記は、このチュートリアルを通して行う変更を理解する助けとなる、
wiki アプリケーションの設計の簡単な概要です。


.. Overall

概要
-------

.. We choose to use ``reStructuredText`` markup in the wiki text.  Translation
.. from reStructuredText to HTML is provided by the widely used ``docutils``
.. Python module.  We will add this module in the dependency list on the project
.. ``setup.py`` file.

wiki テキストの中で ``reStructuredText`` マークアップを使用することを
選択します。 reStructuredText から HTML への変換は、広く用いられている
docutils Python モジュールによって提供されます。このモジュールを
プロジェクトの ``setup.py`` ファイルの依存リストに加えます。


.. Models

モデル
------

.. We'll be using a SQLite database to hold our wiki data, and we'll be using
.. :term:`SQLAlchemy` to access the data in this database.

wiki データを保持するために SQLite データベースを使用します。また、
このデータベース中のデータにアクセスするために SQLAlchemy を使用します。


.. Within the database, we define a single table named `tables`, whose elements
.. will store the wiki pages.  There are two columns: `name` and `data`.

データベース内では、 `tables` という名の単一のテーブルを定義します。
その要素は wiki ページを格納します。このテーブルには 2 つのカラムあります:
`name` と `data` です。


.. URLs like ``/PageName`` will try to find an element in 
.. the table that has a corresponding name.

``/PageName`` のような URL は、テーブル内で対応する名前を持っている要素
を見つけようとします。


.. To add a page to the wiki, a new row is created and the text
.. is stored in `data`.

wiki にページを追加するために、新しい列が作成され、テキストが `data` に
格納されます。


.. A page named ``FrontPage`` containing the text *This is the front page*, will
.. be created when the storage is initialized, and will be used as the wiki home
.. page.

ストレージが初期化される時にテキスト *This is the front page* を含む
``FrontPage`` という名前のページが作られ、 wiki ホームページとして使用
されます。


.. Views

ビュー
------

.. There will be four views to handle the normal operations of adding and
.. editing wiki pages, and viewing pages and the wiki front page.  Two
.. additional views will handle the login and logout tasks related to security.

ページの追加と編集、ページの閲覧、および wiki フロントページ
という通常動作を扱う4つのビューがあるでしょう。2つの追加のビューが、
セキュリティに関係するログインとログアウトのタスクを扱うでしょう。


.. Security

セキュリティ
------------

.. We'll eventually be adding security to our application.  The components we'll
.. use to do this are below.

私たちは、最終的にアプリケーションにセキュリティを加えます。
このために使用するコンポーネントは下にあります。


.. - USERS, a dictionary mapping users names to their corresponding passwords.

- USERS, ユーザ名から対応するパスワードにマッピングする辞書。


.. - GROUPS, a dictionary mapping user names to a list of groups they belong to.

- GROUPS, ユーザー名から所属するグループのリストにマッピングする辞書。


.. - ``groupfinder``, an *authorization callback* that looks up USERS and
..   GROUPS.  It will be provided in a new *security.py* file.

- ``groupfinder``, USERS と GROUPS を検索する *authorization コールバック* 。
  それは新しい ``security.py`` ファイルの中で提供されます。


.. - An :term:`ACL` is attached to the root :term:`resource`.  Each row below
..   details an :term:`ACE`:

- ACLは ルート :term:`resource` に取り付けられています。
  下記の列のそれぞれは :term:`ACE` を詳述します:


  .. +----------+----------------+----------------+
  .. | Action   | Principal      | Permission     |
  .. +==========+================+================+
  .. | Allow    | Everyone       | View           |
  .. +----------+----------------+----------------+
  .. | Allow    | group:editors  | Edit           |
  .. +----------+----------------+----------------+

  +------------+----------------+----------------+
  | アクション | Principal      | Permission     |
  +============+================+================+
  | 許可       | Everyone       | View           |
  +------------+----------------+----------------+
  | 許可       | group:editors  | Edit           |
  +------------+----------------+----------------+


.. - Permission declarations are added to the views to assert the security
..   policies as each request is handled.

- それぞれのリクエストが扱われる度にセキュリティポリシーを検査するために、
  パーミッション宣言がビューに追加されます。


.. Summary

まとめ
-------

.. The URL, actions, template and permission associated to each view are
.. listed in the following table:

各ビューに関連した URL、アクション、テンプレートおよびパーミッションが
次のテーブルにリストされます:


.. +----------------------+-----------------------+-------------+------------+------------+
.. | URL                  |  Action               |  View       |  Template  | Permission |
.. |                      |                       |             |            |            |
.. +======================+=======================+=============+============+============+
.. | /                    |  Redirect to          |  view_wiki  |            |            |
.. |                      |  /FrontPage           |             |            |            |
.. +----------------------+-----------------------+-------------+------------+------------+
.. | /PageName            |  Display existing     |  view_page  |  view.pt   |  view      |
.. |                      |  page [2]_            |  [1]_       |            |            |
.. |                      |                       |             |            |            |
.. |                      |                       |             |            |            |
.. |                      |                       |             |            |            |
.. +----------------------+-----------------------+-------------+------------+------------+
.. | /edit_page/PageName  |  Display edit form    |  edit_page  |  edit.pt   |  edit      |
.. |                      |  with existing        |             |            |            |
.. |                      |  content.             |             |            |            |
.. |                      |                       |             |            |            |
.. |                      |  If the form was      |             |            |            |
.. |                      |  submitted, redirect  |             |            |            |
.. |                      |  to /PageName         |             |            |            |
.. +----------------------+-----------------------+-------------+------------+------------+
.. | /add_page/PageName   |  Create the page      |  add_page   |  edit.pt   |  edit      |
.. |                      |  *PageName* in        |             |            |            |
.. |                      |  storage,  display    |             |            |            |
.. |                      |  the edit form        |             |            |            |
.. |                      |  without content.     |             |            |            |
.. |                      |                       |             |            |            |
.. |                      |  If the form was      |             |            |            |
.. |                      |  submitted,           |             |            |            |
.. |                      |  redirect to          |             |            |            |
.. |                      |  /PageName            |             |            |            |
.. +----------------------+-----------------------+-------------+------------+------------+
.. | /login               |  Display login form.  |  login      |  login.pt  |            |
.. |                      |                       |             |            |            |
.. |                      |  If the form was      |             |            |            |
.. |                      |  submitted,           |             |            |            |
.. |                      |  authenticate.        |             |            |            |
.. |                      |                       |             |            |            |
.. |                      |  - If authentication  |             |            |            |
.. |                      |    successful,        |             |            |            |
.. |                      |    redirect to the    |             |            |            |
.. |                      |    page that we       |             |            |            |
.. |                      |    came from.         |             |            |            |
.. |                      |                       |             |            |            |
.. |                      |  - If authentication  |             |            |            |
.. |                      |    fails, display     |             |            |            |
.. |                      |    login form with    |             |            |            |
.. |                      |    "login failed"     |             |            |            |
.. |                      |    message.           |             |            |            |
.. |                      |                       |             |            |            |
.. +----------------------+-----------------------+-------------+------------+------------+
.. | /logout              |  Redirect to          |  logout     |            |            |
.. |                      |  /FrontPage           |             |            |            |
.. +----------------------+-----------------------+-------------+------------+------------+

+----------------------+-----------------------+-------------+--------------+----------------+
| URL                  |  アクション           |  ビュー     | テンプレート | パーミッション |
|                      |                       |             |              |                |
+======================+=======================+=============+==============+================+
| /                    |  /FrontPage に        |  view_wiki  |              |                |
|                      |  リダイレクト         |             |              |                |
+----------------------+-----------------------+-------------+--------------+----------------+
| /PageName            |  既存のページを       |  view_page  |  view.pt     |  view          |
|                      |  表示 [2]_            |  [1]_       |              |                |
|                      |                       |             |              |                |
|                      |                       |             |              |                |
|                      |                       |             |              |                |
+----------------------+-----------------------+-------------+--------------+----------------+
| /edit_page/PageName  |  既存の内容で         |  edit_page  |  edit.pt     |  edit          |
|                      |  編集フォームを       |             |              |                |
|                      |  表示。               |             |              |                |
|                      |                       |             |              |                |
|                      |  フォームが送信       |             |              |                |
|                      |  されたら /PageName   |             |              |                |
|                      |  にリダイレクト       |             |              |                |
+----------------------+-----------------------+-------------+--------------+----------------+
| /add_page/PageName   |  ストレージに         |  add_page   |  edit.pt     |  edit          |
|                      |  *PageName* を        |             |              |                |
|                      |  作成して空の内容で   |             |              |                |
|                      |  編集フォームを       |             |              |                |
|                      |  表示。               |             |              |                |
|                      |                       |             |              |                |
|                      |  フォームが送信       |             |              |                |
|                      |  されたら /PageName   |             |              |                |
|                      |  にリダイレクト       |             |              |                |
+----------------------+-----------------------+-------------+--------------+----------------+
| /login               |  ログインフォームを   |  login      |  login.pt    |                |
|                      |  表示。               |             |              |                |
|                      |                       |             |              |                |
|                      |  フォームが送信       |             |              |                |
|                      |  されたら認証。       |             |              |                |
|                      |                       |             |              |                |
|                      |  - 認証が成功したら   |             |              |                |
|                      |    元来たページに     |             |              |                |
|                      |    リダイレクト       |             |              |                |
|                      |                       |             |              |                |
|                      |  - 認証が失敗したら   |             |              |                |
|                      |    "login failed"     |             |              |                |
|                      |    というメッセージと |             |              |                |
|                      |    ログインフォームを |             |              |                |
|                      |    表示               |             |              |                |
|                      |                       |             |              |                |
+----------------------+-----------------------+-------------+--------------+----------------+
| /logout              |  /FrontPage に        |  logout     |              |                |
|                      |  リダイレクト         |             |              |                |
+----------------------+-----------------------+-------------+--------------+----------------+

.. .. [1] This is the default view for a Page context
..        when there is no view name.
.. .. [2] Pyramid will return a default 404 Not Found page
..        if the page *PageName* does not exist yet.
.. .. [3] pyramid.exceptions.Forbidden is reached when a
..        user tries to invoke a view that is
..        not authorized by the authorization policy.

.. [1] これはビュー名がない場合のページコンテキストに対するデフォルト
       ビューです。
.. [2] Pyramid は、ページ *PageName* がまだ存在しない場合に、
       デフォルトの 404 Not Found ページを返すでしょう。
.. [3] ユーザが認可ポリシーによって許可されないビューを起動しようとした
       場合、 pyramid.exceptions.Forbidden に到達します。

