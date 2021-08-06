.. _lv1-clone:

=========================================
他の人のソースを取得する
=========================================

`GitHub <https://github.io>`_ 等のリポジトリサイトから、任意の公開リポジトリを引っ張ってこれるようになりましょう。
グループ開発などでリポジトリを共有するときなどに真っ先にこれが必要になります。

1. 取得元を確認する
2. cloneを行う

取得元を確認する
============================

取得元に関しては、大雑把に2つのケースがあります。

- プロジェクトサイトを教えてもらい、そこから取得
- clone用のURLを直接教えてもらう

プロジェクトサイトがGithubなどのコード公開用のサイトであれば、その中から確認できます。
サンプルとして、Hello World集的なものをちょっと作ってます。

- `densuke-st/hello-world <https://github.com/densuke-st/hello-world>`_

.. figure:: images/01-densuke-st-hello-world.png

    densuke-st/hello-world

Githubの場合は、コード取得として緑色の :guilabel:`Code` 部分から、取得が可能です。

.. figure:: images/02-click-code.png

    Codeボタンを押したとき

通常は "HTTPS" タブで確認できるURLを使うと良いでしょう。

.. note::

    GitHub CLI(:command:`gh` コマンド)をインストールした場合、コマンドラインから少しだけ楽に操作できます。
    詳しくは `GitHub CLI <https://cli.github.com/>`_ を参照してください。

cloneを行う
====================

取得元URLが確認できたら、cloneを行います。
clone操作は指定されたURLに存在する「リポジトリ」のクローンをローカルに作成する操作です。

コマンドラインで行う
------------------------------

コマンドラインから行う場合、対象となるディレクトリにて :command:`git` を実行します。
ここでは `densuke-st/hello-world <https://github.com/densuke-st/hello-world>`_ のclone URLを用いて行います。

.. code-block:: console
    :caption: git clone処理

    # Linux/macOS
    $ git clone https://github.com/densuke-st/hello-world.git

    # Windows(PowerShell)
    PS> git clone https://github.com/densuke-st/hello-world.git

.. figure:: images/03-git-clone.png

    git cloneによるコード取得

.. warning::

    この作業により、ディレクトリ :file:`hello-world` ができあがります。
    既にこのディレクトリが存在している場合は失敗します。
    既存のディレクトリを削除するか、名前を変えるなどの対策が必要です。

    .. code-block:: console

        $ git clone https://github.com/densuke-st/hello-world.git
        fatal: destination path 'hello-world' already exists and is not an empty directory.


あとはできあがったディレクトリの中を好きに探索してください。
なお、 `densuke-st/hello-world <https://github.com/densuke-st/hello-world>`_ の :file:`Java/Maven` ディレクトリはEclipseにてインポート可能にしてます。

このように、取得用URLがわかれば、自分の手元にコードのコピーを持つことが可能です。

IDEで行う
----------------------

.. todo:: Eclipse等の作業について書いておく

zipと何が違うの?
===========================

これだけだと :guilabel:`Code` にてzipファイルを落としたのと変わらないと思います。
でも実は大きく違います。

- 履歴が見られます(:command:`git log`)
    .. figure:: images/04-git-log.png

- 誰かが更新したときに、更新分を取得可能です(pullの挙動は後ほど入ります)
     .. code-block:: console
        :caption: コードの更新(git pull)

        $ git pull
        remote: Enumerating objects: 21, done.
        remote: Counting objects: 100% (21/21), done.
        remote: Compressing objects: 100% (3/3), done.
        remote: Total 11 (delta 2), reused 11 (delta 2), pack-reused 0
        Unpacking objects: 100% (11/11), 809 bytes | 101.00 KiB/s, done.
        From https://github.com/densuke-st/hello-world
        dcd3b13..826094c  development -> origin/development
        Updating dcd3b13..826094c
        Fast-forward
        Java/Maven/helloworld/src/main/java/jp/example/App.java | 2 +-
        1 file changed, 1 insertion(+), 1 deletion(-)