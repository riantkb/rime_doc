クイックスタート
================

ここでは、あらかじめ用意された設定例を使って Rime を実行し、Rime の機能の概略を紹介します。

以下では、前節で ~/src/rime に Rime をダウンロードしたと仮定します。

.. code-block:: bash

    $ cd ~/src/rime

このディレクトリには次のようなファイルが存在するはずです(最新版ではもう少しファイルが多い可能性もあります)。

.. code-block:: bash

    $ ls -l
    total 12
    -rw-r--r-- 1 nya nya    0 May 11 23:55 README
    drwxr-xr-x 3 nya nya 4096 May 11 23:55 example
    drwxr-xr-x 6 nya nya 4096 May 11 23:55 rime
    -rwxr-xr-x 1 nya nya 1215 May 11 23:55 rime.py

example ディレクトリには設定例が用意されていますので、そこに移動しましょう。

.. code-block:: bash

    $ cd example
    $ ls -l
    total 8
    -rw-r--r-- 1 nya nya   96 May 11 23:55 PROJECT
    drwxr-xr-x 6 nya nya 4096 May 11 23:55 a+b
    lrwxrwxrwx 1 nya nya    7 May 12 00:10 rime -> ../rime
    lrwxrwxrwx 1 nya nya   10 May 11 23:55 rime.py -> ../rime.py

このディレクトリ以下には、コンテストの問題・解答・入出力ジェネレータなど全てに関する設定が保存されています。このようなディレクトリを **プロジェクトディレクトリ** と呼びます。

プロジェクトディレクトリには Rime 本体とプロジェクト設定ファイル(PROJECT)、そして問題ディレクトリを置きます。問題ディレクトリは通常複数あるでしょうが、この例では一問だけ (a+b) 用意されています。プロジェクトディレクトリ以下には Rime が認識するもの以外のディレクトリやファイルがあっても問題ありません。なお、このプロジェクト例では簡単のため Rime 本体をシンボリックリンクとして置いてありますが、通常のプロジェクトではファイルをまるごとコピーして置くと良いでしょう。

早速 Rime を実行してみましょう。

.. code-block:: bash

    $ ./rime.py test
    [ GENERATE ] a+b/tests: generator.py
    [ VALIDATE ] a+b/tests: OK
    [ COMPILE  ] a+b/cpp-correct
    [  REFRUN  ] a+b/cpp-correct
    [ COMPILE  ] a+b/cpp-TLE
    [   TEST   ] a+b/cpp-TLE: 11-maximum.in: Time Limit Exceeded
    [ COMPILE  ] a+b/cpp-WA-multiply
    [   TEST   ] a+b/cpp-WA-multiply: Expectedly failed all challenge cases
    [   TEST   ] a+b/cpp-correct: max 0.00s, acc 0.09s
    [   TEST   ] a+b/python-correct: max 0.04s, acc 0.72s

    Test Summary:
    a+b ... 4 solutions, 24 tests
      cpp-correct      OK  max 0.00s, acc 0.09s
      python-correct   OK  max 0.04s, acc 0.72s
      cpp-TLE          OK  11-maximum.in: Time Limit Exceeded
      cpp-WA-multiply  OK  Expectedly failed all challenge cases

    Error Summary:
    Total 0 errors, 0 warnings

このコマンドは数秒で終了するはずです。この間に Rime は次の処理を行いました。

- 問題ディレクトリ a+b 以下にある 4 つの解答 (cpp-correct, python-correct, cpp-TLE, cpp-WA-multiply) のコンパイル
- 入出力ディレクトリ a+b/tests 以下にある入力生成器と入力検証器のコンパイル
- 入力生成器を使ったランダム入力データの生成
- 入力検証器を使った全入力データのフォーマットチェック
- 解答プログラム a+b/cpp-correct に入力データを入れて走らせ、リファレンスとなる出力データを生成
- 解答プログラム a+b/python-correct を走らせ、リファレンス出力データと同じ出力を出すことを確認
- 誤答プログラム a+b/cpp-WA-multiply を走らせ、間違った出力を出すことを確認
- 誤答プログラム a+b/cpp-TLE を走らせ、指定したタイムリミット (1秒) 内に終了しないことを確認

Rime が正しく動いていることを確認するため、試しに解答プログラムにバグを入れてみましょう。名前からお察しのとおり、問題 a+b は標準入力から二つの数を受け取り、その和を標準出力に書き出すプログラムを書け、という問題です。python-correct プログラムは次のようなコードになっています

.. code-block:: python

    # $ cat a+b/python-correct/main.py
    #!/usr/bin/python

    import sys

    def main():
      a, b = map(int, sys.stdin.read().strip().split())
      print a + b

    if __name__ == '__main__':
      main()

これを、掛け算をするプログラムに書き換えます。

.. code-block:: python

    # $ vi a+b/python-correct/main.py
    # ...
    # $ cat a+b/python-correct/main.py
    #!/usr/bin/python

    import sys

    def main():
      a, b = map(int, sys.stdin.read().strip().split())
      print a * b  # i can haz moar?

    if __name__ == '__main__':
      main()

再び Rime を実行してみましょう。

.. code-block:: bash

    $ ./rime.py test
    [   TEST   ] a+b/cpp-TLE: 11-maximum.in: Time Limit Exceeded
    [   TEST   ] a+b/cpp-WA-multiply: Expectedly failed all challenge cases
    [   TEST   ] a+b/cpp-correct: max 0.01s, acc 0.09s
    ERROR: a+b/python-correct: 00-sample1.in: Wrong Answer
      judge log: /home/nya/src/rime/example/a+b/rime-out/python-correct/00-sample1.judge
    [   TEST   ] a+b/python-correct: 00-sample1.in: Wrong Answer

    Test Summary:
    a+b ... 4 solutions, 24 tests
      cpp-correct      OK  max 0.01s, acc 0.09s
      python-correct  FAIL 00-sample1.in: Wrong Answer
      cpp-TLE          OK  11-maximum.in: Time Limit Exceeded
      cpp-WA-multiply  OK  Expectedly failed all challenge cases

    Error Summary:
    ERROR: a+b/python-correct: 00-sample1.in: Wrong Answer
      judge log: /home/nya/src/rime/example/a+b/rime-out/python-correct/00-sample1.judge
    Total 1 errors, 0 warnings

python-correct が 00-sample1.in で間違っていた、とのメッセージが出ています。具体的にどのような出力をしたのかはジャッジログファイルに残っています。

.. code-block:: bash

    $ cat /home/nya/src/rime/example/a+b/rime-out/python-correct/00-sample1.judge
    --- /home/nya/src/rime/example/a+b/rime-out/tests/00-sample1.diff       2012-05-12 00:54:54.463139744 +0900
    +++ /home/nya/src/rime/example/a+b/rime-out/python-correct/00-sample1.out       2012-05-12 01:08:55.827436428 +0900
    @@ -1 +1 @@
    -7
    +12
