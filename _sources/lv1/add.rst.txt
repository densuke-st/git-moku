.. _lv1-add:

====================
リポジトリ操作
====================


ファイルの登録
=========================

ワークツリーとなったこのディレクトリ上にPythonのコードをひとつ作ってください。
作成方法は問いません。まずは基本となるコマンドラインからです。

.. code-block:: console
    :caption: コマンドラインからPythonスクリプトを作成(UNIX)

    $ echo "print('Hello')" > sample1.py
    $ python sample1.py
    Hello

.. code-block:: console
    :caption: コマンドラインからPythonスクリプトを作成(PowerShell)

    PS> echo "print('Hello')" | Out-File -Encoding utf8 sample1.py
    PS> python sample1.py
    Hello

この状態で、以下のコマンドを使ってみてください。

.. code-block:: console
    :caption: ファイル状態の確認
    :linenos:

    $ git status
    On branch development

    No commits yet

    Untracked files:
    (use "git add <file>..." to include in what will be committed)
            sample1.py

    nothing added to commit but untracked files present (use "git add" to track)

- 2行目: 「ブランチ」はコードの変更の流れに対する名称です、現在は `main` が主流ですが、先生は `development` にしています
- 4行目: 現在はまだ「コミットされていません」とのこと、コミットとは?
- 6行目: Un-track-ed(まだ **追跡** されていない)という状態、git的に野良なものであり、これを **追加** することで変更の **追跡** が行われるようになります

ということで、追跡してもらうためにも追加処理を行います。

.. code-block:: console
    :caption: sample1.pyの追加
    :linenos:
    :emphasize-lines: 1

    $ git add sample1.py
    $ git status
    On branch development

    No commits yet

    Changes to be committed:
    (use "git rm --cached <file>..." to unstage)
            new file:   sample1.py

追加後に再度 :command:`git status` したところ、少し変化があります。

- 1行目: 追加しました
- 7行目: 「コミットされるべき変更があります」という旨
- 9行目: 新規ファイルとしてのsample1.py

このとき、ディレクトリ・ファイル達はこのような関係で構成されます。

.. figure:: images/repo-local-01.png

    add前の状態

addすると、リポジトリ域の中に用意されている **ステージ** という領域に追加したファイルのコピーが配置されます。

.. figure:: images/repo-local-02.png

    add後の状態

ステージ域は **次にコミットされるべき情報** が配置されます。
逆に考えれば、敢えてステージに入れなければコミットされないということになります。

コミット
======================

コミットはステージに上げている状態のファイルをまとめて **ひとつの状態として固定化する** 行為となります。
一種のセーブポイントと考えて良いでしょう。

このとき **なんのためのコミットか** をきちんと書く必要がありますが、最低1行「なんのためか」を書けばさしあたりなんとかなります。
これはコミット処理を行う :command:`git commit` の時に :command:`-m メッセージ` で1行コメントを渡せます。

.. code-block:: console
    :caption: コミット
    :emphasize-lines: 1

    $ git commit -m "練習用Pythonスクリプトの作成"
    [development (root-commit) ad05401] 練習用Pythonスクリプトの作成
     1 file changed, 1 insertion(+)
     create mode 100644 sample1.py

この操作により、ステージに入っていたファイル :file:`sample1.py` は各種メタデータと一緒にまとめられてリポジトリにその時の状態で固定化されます。

この際、コミットIDと呼ばれる固有値(ハッシュ)が生成され、ステージ状態のスナップショットに対するアドレスとして機能するようになります。
上記の出力では `ad05401` が該当しています。

.. figure:: images/repo-local-03.png

    commit後の状態

vscodeでの操作
------------------------

vscodeでも考え方は同じです。
リポジトリ内でファイルを作ると、ソース管理のアイコンに対象となり得るファイル数が通知で出力されます。

.. figure:: images/09-newfile-vscode.png

    vscode上で新規(更新)が認識されている状態

ソース管理に切り替えると、新規か更新されたファイルがリストアップされます。

.. figure:: images/10-files-vscode.png

    作ったばかりのファイルのため、まだUntrack状態のファイルとして認識されている

ということで、+ボタンを使って対象ファイルをステージに上げておきます。

.. figure:: images/11-staged-vscode.png

    ステージに上げた状態

あとはコミットメッセージを書いて登録すればリポジトリにコミットされます。

.. figure:: images/12-commitmsg-vscode.png

    メッセージを書いた状態、右上のチェック(✓)記号かCtrl+Enterでステージをコミットします

コミット後、下部のcommitsを展開すると、コミット履歴として確認ができます。

.. figure:: images/13-commits-vscode.png

    コミット状態の確認

ソース管理では、コミット状態のグラフとなるcommits意外に、現在操作中のファイルの変更履歴を見るfile historyなど、それなりに役立つ情報が確認できます。ただしこれらはgitの仕組みがある程度理解できないと使えないので、参考程度に見るようにしておきましょう。

ファイルを編集してみる
=============================

:file:`sample1.py` を編集し、出力内容を変更してみましょう。

.. code-block:: python
    :caption: sample1.pyの修正(出力内容変更)

    print('Hello, World')

この場合、ステージ上に前の状態の :file:`sample1.py` ファイルが残っており、ワークツリー(:file:`.git` の外)の :file:`sample1.py` との間で内容が違うということになります。
この状態をgitは検出できます(:command:`git status`)。

.. code-block:: console
    :caption: 変更の検出
    :emphasize-lines: 6

    $ git status
    On branch development
    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git restore <file>..." to discard changes in working directory)
            modified:   sample1.py

    no changes added to commit (use "git add" and/or "git commit -a")

6行目にあるように「変更されている(modified)」というマークが付いています。
ステージ上に残っているものとの比較も可能です(:command:`git diff`)

.. code-block:: console
    :caption: diffの取得

    $ git diff
    diff --git a/sample1.py b/sample1.py
    index c324c2e..f93eac0 100644
    --- a/sample1.py
    +++ b/sample1.py
    @@ -1 +1 @@
    -print('Hello')
    +print('Hello, World')

diffはunified diff形式となっており、 "-"の部分が"+"に置換されることを示しています。

.. figure:: images/repo-local-04.png

    ワークツリー上を変更するとステージと食い違うことになる

この状態を登録するために、追加してコミットしてみます。

.. code-block:: console
    :caption: 変更したファイルの追加コミット

    # 追加する
    $ git add sample1.py

    # 状態確認 → 「次にコミットされる変更」に登録
    $ git status
    On branch development
    Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
            modified:   sample1.py

    # diffしても出ない(ステージ上とワークツリーが同じになったため)
    $ git diff
    # コミット
    $ git commit -m "世界よこんにちは"
    [development 43837ea] 世界よこんにちは
    1 file changed, 1 insertion(+), 1 deletion(-)

.. figure:: images/repo-local-05.png

    修正したファイルのコミット

同様の操作はvscodeでももちろん行えます。
:file:`sample1.py` の書き換えをさらにvscode上で行い、適宜追加・コミットを試してみましょう。


今度は自分でやってみよう
=====================================

今度は自分でやってみましょう。

1. :file:`sample2.py` を作ってみましょう、内容は何でも良いです(コードとしてエラーでない範囲で)。
2. :file:`sample2.py` をコミットしてみましょう。コミットメッセージはお好きにどうぞ。
3. このとき、ワークツリー状態とステージ、(狭義の)リポジトリ部分がどうなっているかを図示してみましょう。

vscodeやEclipseでも同様の操作で行えますから、自分でファイルを作ってコミットしていきましょう。
