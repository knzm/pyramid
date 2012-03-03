.. The Pyramid Web Application Development Framework

.. _index:

=================================================
Pyramid Web アプリケーション開発フレームワーク
=================================================

.. :app:`Pyramid` is a small, fast, down-to-earth Python web application
.. development framework.  It is developed as part of the `Pylons Project
.. <http://docs.pylonsproject.org/>`_.  It is licensed under a `BSD-like license
.. <http://repoze.org/license.html>`_.

:app:`Pyramid` は、小さく、速く、堅実 (down-to-earth) な Python ウェブ
アプリケーション開発フレームワークです。 Pyramid は Pylons プロジェクトの
一部として開発されています。 `BSD-like なライセンス <http://repoze.org/license.html>`_
の下でライセンスされています。


.. Here is one of the simplest :app:`Pyramid` applications you can make:

最も単純な :app:`Pyramid` アプリケーションはこのようになります:


.. literalinclude:: narr/helloworld.py


.. When saved to ``helloworld.py``, the above application can be run via:

このファイルを ``helloworld.py`` として保存したら、上記のアプリケーションは
次のようにして実行することができます:


.. code-block:: text

   $ easy_install pyramid
   $ python helloworld.py


.. When you visit ``http://localhost:8080/hello/world`` in a browser, you will
.. see the text ``Hello, world!``.

ブラウザで ``http://localhost:8080/hello/world`` を訪れれば、
``Hello, world!`` というテキストが見えるはずです。


.. See :ref:`firstapp_chapter` for a full explanation of how this application
.. works. Read the :ref:`html_narrative_documentation` to understand how
.. :app:`Pyramid` is designed to scale from simple applications like this to
.. very large web applications.

このアプリケーションがどのようにして動くのかについての完全な説明は、
:ref:`firstapp_chapter` を参照してください。 :app:`Pyramid` がこの
ような単純なアプリケーションから非常に大きなウェブアプリケーションまで
スケールするためにどのように設計されているかを理解するには
:ref:`html_narrative_documentation` を読んでください。


.. Front Matter

まえがき
============

.. toctree::
   :maxdepth: 1

   copyright.rst
   conventions.rst


.. "What's New" Documents

"What's New" ドキュメント
=========================

.. toctree::
   :maxdepth: 1

   whatsnew-1.3
   whatsnew-1.2
   whatsnew-1.1
   whatsnew-1.0


.. Narrative documentation

.. _html_narrative_documentation:

マニュアル
=======================

.. Narrative documentation in chapter form explaining how to use
.. :app:`Pyramid`.

:app:`Pyramid` を使用する方法について説明した、章形式のマニュアル
(Narrative documentation) です。


.. toctree::
   :maxdepth: 2

   narr/introduction
   narr/install
   narr/firstapp
   narr/configuration
   narr/project
   narr/startup
   narr/router
   narr/urldispatch
   narr/views
   narr/renderers
   narr/templates
   narr/viewconfig
   narr/assets
   narr/webob
   narr/sessions
   narr/events
   narr/environment
   narr/logging
   narr/paste
   narr/commandline
   narr/i18n
   narr/vhosting
   narr/testing
   narr/resources
   narr/hellotraversal
   narr/muchadoabouttraversal
   narr/traversal
   narr/security
   narr/hybrid
   narr/hooks
   narr/introspector
   narr/extending
   narr/advconfig
   narr/extconfig
   narr/scaffolding
   narr/threadlocals
   narr/zca


.. Tutorials

チュートリアル
==============

.. Detailed tutorials explaining how to use :app:`Pyramid` to build
.. various types of applications and how to deploy :app:`Pyramid`
.. applications to various platforms.

様々なタイプのアプリケーションを構築するために :app:`Pyramid` を使用
する方法や、 :app:`Pyramid` アプリケーションを様々なプラットフォームへ
展開する方法について説明した、詳細なチュートリアルです。


.. toctree::
   :maxdepth: 2

   tutorials/wiki2/index.rst
   tutorials/wiki/index.rst
   tutorials/bfg/index.rst
   tutorials/modwsgi/index.rst


.. Reference Material

リファレンス資料
==================

.. Reference material includes documentation for every :app:`Pyramid` API.

リファレンス資料には、すべての :app:`Pyramid` API に対するドキュメント
が含まれています。


.. toctree::
   :maxdepth: 2

   api


.. Detailed Change History

詳細な変更履歴
=======================

.. toctree::
   :maxdepth: 1

   changes


.. Design Documentation

設計ドキュメント
====================

.. toctree::
   :maxdepth: 1

   designdefense


.. Sample Applications

サンプルアプリケーション
========================

.. `cluegun <https://github.com/Pylons/cluegun>`_ is a simple pastebin
.. application based on Rocky Burt's `ClueBin
.. <http://pypi.python.org/pypi/ClueBin/0.2.3>`_.  It demonstrates form
.. processing, security, and the use of :term:`ZODB` within a :app:`Pyramid`
.. application.  Check this application out via:

`cluegun <https://github.com/Pylons/cluegun>`_ は、 Rocky Burt の
`ClueBin <http://pypi.python.org/pypi/ClueBin/0.2.3>`_ を基にした単純な
pastebin アプリケーションです。 Pyramid アプリケーションでの
フォーム処理、セキュリティ、 :term:`ZODB` の使用を実証します。
次のようにしてこのアプリケーションをチェックアウトしてください:


.. code-block:: text

  git clone git://github.com/Pylons/cluegun.git


.. `virginia <https://github.com/Pylons/virginia>`_ is a very simple dynamic
.. file rendering application.  It is willing to render structured text
.. documents, HTML documents, and images from a filesystem directory.
.. It's also a good example of :term:`traversal`. An
.. earlier version of this application runs the `repoze.org
.. <http://repoze.org>`_ website.  Check this application out via:

`virginia <https://github.com/Pylons/virginia>`_ は非常に単純な動的
ファイル表示アプリケーションです。ファイルシステム上のディレクトリから、
構造化テキストドキュメント、HTML ドキュメント、画像を表示しようとします。
これは :term:`traversal` のよい例でもあります。このアプリケーションの
初期のバージョンは repoze.org ウェブサイトを動かしていました。次のように
してこのアプリケーションをチェックアウトしてください:


.. code-block:: text

  git clone git://github.com/Pylons/virginia.git


.. `shootout <https://github.com/Pylons/shootout>`_ is an example "idea
.. competition" application by Carlos de la Guardia and Lukasz Fidosz.  It
.. demonstrates :term:`URL dispatch`, simple authentication, integration
.. with `SQLAlchemy <http://www.sqlalchemy.org/>`_ and ``pyramid_simpleform``.
.. Check this application out of version control via:

`shootout <https://github.com/Pylons/shootout>`_ は、 Carlos de la
Guardia と Lukasz Fidosz による「アイデア競争」アプリケーションの例です。
:term:`URL dispatch` 、簡易認証、 `SQLAlchemy <http://www.sqlalchemy.org/>`_
と ``pyramid_simpleform`` の統合を実証します。次のようにしてこの
アプリケーションをバージョン管理からチェックアウトしてください:


.. code-block:: text

  git clone git://github.com/Pylons/shootout.git


.. Older Sample Applications (repoze.bfg)

古いサンプルアプリケーション (repoze.bfg)
=========================================

.. note::

   .. These applications are for an older version of :app:`Pyramid`, which was
   .. named :mod:`repoze.bfg`.  They won't work unmodified under Pyramid, but
   .. might provide useful clues.

   これらのアプリケーションは、 :app:`Pyramid` のより古いバージョンである
   repoze.bfg 向けです。変更なしでは Pyramid の下で動作しませんが、
   有用な手掛かりを提供するかもしれません。


.. `bfgsite <http://svn.repoze.org/bfgsite/trunk>`_ is the software which
.. runs the `bfg.repoze.org <http://bfg.repoze.org>`_ website.  It
.. demonstrates integration with Trac, and includes several
.. mini-applications such as a pastebin and tutorial engine.  Check a
.. buildout for this application out of Subversion via:

`bfgsite <http://svn.repoze.org/bfgsite/trunk>`_ は bfg.repoze.org
ウェブサイトを動かしているソフトウェアです。 Trac との統合を実証し、
pastebin やチュートリアルエンジンのようないくつかのミニアプリケーションを
含んでいます。次のようにして Subversion からこのアプリケーションの
buildout をチェックアウトしてください:


.. code-block:: text

  svn co http://svn.repoze.org/buildouts/bfgsite/ bfgsite_buildout


.. `KARL <http://karlproject.org>`_ is a moderately-sized application
.. (roughly 70K lines of Python code) built on top of :mod:`repoze.bfg`
.. and other Repoze software.  It is an open source web system for
.. collaboration, organizational intranets, and knowledge management, It
.. provides facilities for wikis, calendars, manuals, searching, tagging,
.. commenting, and file uploads.  See the `KARL site
.. <http://karlproject.org>`_ for download and installation details.

`KARL <http://karlproject.org>`_ は、 `repoze.bfg` とその他の Repoze
ソフトウェアの上に構築された 中程度のサイズ (およそ 70K 行の Python コード)
のアプリケーションです。それは、共同作業、組織的なイントラネット、
ナレッジマネジメント用のオープンソース・ウェブシステムで、
Wiki 、カレンダー、マニュアル、検索、タグ付け、コメント、ファイル
アップロード機能を提供します。ダウンロードとインストールの詳細については、
`KARL サイト <http://karlproject.org>`_ を参照してください。


.. Support and Development

サポートと開発
=======================

.. The `Pylons Project web site <http://pylonsproject.org/>`_ is the main online
.. source of :app:`Pyramid` support and development information.

`Pylons Project ウェブサイト <http://pylonsproject.org/>`_ は、
:app:`Pyramid` のサポートおよび開発情報についてのオンラインの主な情報源です。


.. To report bugs, use the `issue tracker
.. <http://github.com/Pylons/pyramid/issues>`_.

バグを報告するためには、 `issue tracker
<http://github.com/Pylons/pyramid/issues>`_ を使用してください。


.. If you've got questions that aren't answered by this documentation,
.. contact the `Pylons-devel maillist
.. <http://groups.google.com/group/pylons-devel>`_ or join the `#pyramid
.. IRC channel <irc://irc.freenode.net/#pyramid>`_.

このドキュメントが答えていない質問がある場合は、 `Pylons-devel メーリン
グリスト <http://groups.google.com/group/pylons-devel>`_ に連絡するか、
`#pyramid IRCチャンネル <irc://irc.freenode.net/#pyramid>`_ に join
してください。


.. Browse and check out tagged and trunk versions of :app:`Pyramid` via
.. the `Pyramid GitHub repository <http://github.com/Pylons/pyramid/>`_.
.. To check out the trunk via ``git``, use this command:

`Pyramid GitHub リポジトリ <http://github.com/Pylons/pyramid/>`_ 経由で
:app:`Pyramid` のタグや trunk バージョンを閲覧、チェックアウトしてください。
``git`` を使って trunk をチェックアウトするためには、このコマンドを使用してください:


.. code-block:: text

  git clone git@github.com:Pylons/pyramid.git


.. To find out how to become a contributor to :app:`Pyramid`, please see the
.. `contributor's section of the documentation
.. <http://docs.pylonsproject.org/index.html#contributing>`_.

Pyramid の貢献者になる方法を理解するためには、 `ドキュメントの貢献者の
セクション <http://pylons-ja.readthedocs.org/en/latest/index.html#id5>`_ を
参照してください。


.. Index and Glossary

索引と用語集
==================

* :ref:`glossary`
* :ref:`genindex`
* :ref:`search`


.. add glossary, foreword, and latexindex in a hidden toc to avoid warnings

.. toctree::
   :hidden:

   glossary
   foreword.rst
   latexindex.rst

