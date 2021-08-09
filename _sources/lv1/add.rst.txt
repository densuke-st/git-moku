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

Eclipseでの操作
-------------------------

EclipseではJavaで行ってみましょう。
さしあたり、git下にあるプロジェクトにおいて、Javaのクラス(ここでは :code:`Sample1`)を通常の手順で作成するとします(:code:`main` スタブ有り)。

.. figure:: images/32-newclass-eclipse.png

    新規クラス(:code:`Sample1`)を作成したところ

このファイルを作成すると(既に初期状態で保存されている)、パッケージ・エクスプローラーにてはてな(?)マークが付きます。

.. figure:: images/33-hatena.png

    新規のファイルに付いたはてなマーク

この状態は、gitにとって新規(untracked)であることを示しています。
作ったばかりのファイルは当然gitでの追跡対象にはまだ含まれていません。

ということでこれを登録してみます。方法はいくつかありますが、いずれもコンテキストメニューの  :menuselection:`チーム` から行います。

- 該当ファイルを「索引に追加」することでステージに上げる
- 「コミット」でまとめてステージに上げていきなりコミットへ

:code:`Sample1` については、ステージに上げるだけにしておきます。
もうひとつクラスを作り、直接コミットへ飛ばして違いを見てみましょう。

.. figure:: images/34-add-eclipse.png

    ステージに上げるとマークが変わる

同様にクラス :code:`Sample2` を作成しておきます(手順は省略)。
こちらは  :menuselection:`チーム --> コミット` を使って操作してみたいと思います。

.. figure:: images/35-staging-eclipse.png

    gitステージングのウィンドウ(タブ)

ステージに上がっていないファイル達がリストアップされています。
この中で次のコミットに含めたいファイルがあれば該当ファイルを選び(複数選択可能)、 :guilabel:`+` にて **ステージされた変更** に移動します。
:guilabel:`++`  を使えばひとまとめで移動させることもできます。
逆に含めたくない(今回のコミットには含めない)のであれば逆に :guilabel:`-`/:guilabel:`--` にて下げることも可能です。

ステージの状態でコミットしようというのであれば、コミットメッセージを書いてコミットすればOKです。
ただし、コミットは現時点では :guilabel:`コミット` となります、
:guilabel:`コミットおよびプッシュ` は使えません。

.. figure:: images/36-commit-eclipse.png

    コミット処理

EclipseのUI部分でざっくりまとめると、

- ファイルやディレクトリにマーク(?, +, *)がついて状態をある程度表示してくれています
- 個別に索引(ステージ)に入れたり、ひとまとめで入れたりすることが可能です
- gitステージングのウィンドウ(タブ)にてコミット処理が行えます

なお、プロジェクト構成のファイル(:file:`.project` や :file:`.classpath`)も適宜変更されますので、Eclipseのプロジェクトとして共有するのであれば、これらもコミットした方が良いでしょう。

- :file:`Sample2.java` をコミットしてみましょう(:file:`Sample1.java` と一緒に)
- :file:`.project`/:file:`.classpath` をコミットしてみましょう(Javaソースとは別でコミット)
- file:`.gitignore` をコミットしてみましょう(これも別で)

.. note::
    gitステージングをよく見ると、 :file:`.gitignore` というものが目に入ると思います。
    このファイルは、gitが管理対象とみなさないファイル(のパターン)を格納するためのもので、作成すると、そのディレクトリおよびサブディレクトリに適用されます。
    ディレクトリであれば末尾に :code:`/` を加えることでディレクトリとその中身を対象から除外できます。
    この仕組みを利用することで **ビルドすれば得られるデータのある場所** については対象外とすることができますし、基本的にそのように構成します。
    Eclipseの中では自動的に :file:`bin` ディレクトリ(裏で自動的にビルドした結果の入る場所)を除外するようにしています。
    必要であればこのファイルの中は書き換えてもらってもかまいませんが、このファイルをもコミットして管理下におくようにしてください。

    そういう意味では、最初のコミットの際に :file:`.project` などと共にコミットしておくのが安全です。

ファイルを編集してみる
=============================

.. note::
    Python(素およびvscode)向けに書いていますが、Eclipse(Java)であっても原則は同じなので、適当にJavaコード的に読み替えて操作してみましょう。

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

同様の操作はvscodeやEclipseでももちろん行えます。
それぞれのワークツリー上にあるファイルを書き換えて、変更されたファイルのみをコミットしてみましょう。

今度は自分でやってみよう
=====================================

今度は自分でやってみましょう。

素でやるかvscodeの方向け
-----------------------------

1. :file:`sample2.py` を作ってみましょう、内容は何でも良いです(コードとしてエラーでない範囲で)。
2. :file:`sample2.py` をコミットしてみましょう。コミットメッセージはお好きにどうぞ。
3. このとき、ワークツリー状態とステージ、(狭義の)リポジトリ部分がどうなっているかを図示してみましょう。

Eclipseの方向け
-----------------------------

1. クラス :code:`Sample3` を作り(中身は問わない)、コミットしてみましょう。
2. 同様に、ワークツリーの状態とステージ、リポジトリ部分がどうなっているかを図辞してみましょう。