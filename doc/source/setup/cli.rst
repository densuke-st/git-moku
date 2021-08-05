.. _setup-cli:

=======================================
素のgitでのセットアップ
=======================================

実はこれが一番簡単な気がします。

.. setup-install-cli:

gitのインストール
==========================

git環境はIDEに付属していることもありますが、素のgitが使えた方が細かいところで役立ちます。
以降のIDE向けへ行く前に準備しておくと良いでしょう。

- `git <https://git-scm.com/>`_

gitの配布サイトへ行き、各OS版を落とすと良いでしょう

Windows環境
    - `ダウンロードページ <https://git-scm.com/download/win>`_ へ行き、32bit版、64bit版を選んでダウンロードしてインストールしてください
        - インストール中の改行コード設定に注意してください(:ref:`setup-win-crlf`)
    - chocolatey使っている方は、そちらで入れると良いでしょう。

      .. code-block:: console

          PS> choco install -y git

macOS環境
    - `ダウンロードページ <https://git-scm.com/download/mac>`_ へ行き、指示に従ってインストールするといいでしょう
    - 普通の人はhomebrewを入れていると思います

      .. code-block:: console

          $ brew install git

    - 残念ながらhomebrew入れてない人は、Binary installer からダウンロードして入れると良いでしょう。

Linux環境
    - Ubuntuなら :command:`apt` で入れればOK

      .. code-block:: console

          $ sudo apt update; sudo apt install -y git

    - RHEL/CentOS系なら :command:`dnf` で入れればOK

      .. code-block:: console

          $ sudo dnf install git

.. _setup-regist:

ユーザー登録
=====================

コミット(記録する行為)ログ作成の際に必須になります。
と言っても別にどこかのサイトに登録するなどではないので、名前とメールアドレスを入れてましょう。

.. code-block:: console
    :caption: ユーザー登録(グローバル)

    $ git config --global user.name "名前(ローマ字・英語表記)"
    $ git config --global user.email "メールアドレス"
    $ git config --list # 内容確認

間違っていたら間違っている部分を再度設定すれば上書き修正できます。

なお、特定のプロジェクトの中で個別の設定を使いたい場合、リポジトリ上でローカル指定も可能です。

.. code-block:: console
    :caption: ユーザー登録(プロジェクトローカル)

    $ git config --local user.name "名前(ローマ字・英語表記)"
    $ git config --local user.email "メールアドレス"
    $ git config --list # 内容確認

.. _setup-win-crlf:

改行コードの扱い(Windows)
=====================================

Windowsは他のOSと違い、改行コードがCRLFのため、他のOSでの開発と混ざったときに改行コードの違いでトラブルの元になります。
そのため、インストール時に改行コード設定をデフォルトから `Checkout as-is, commit Unix-style line encodings` に変更しておくことをおすすめします。

ないしは、 :command:`git config` で修正しておくと良いでしょう(`core.autocrlf`)。

.. code-block:: console
    :caption: 改行コードの設定変更

    $ git config --global core.autocrlf input

