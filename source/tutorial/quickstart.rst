
================
クイックスタート
================

実際に簡単な問題の準備を行うことで、Rime の使い方に慣れていきましょう。


----


作業用ディレクトリの準備
================================

まず、作問準備の作業用ディレクトリを用意しましょう。ディレクトリ名はなんでも良いです。

用意したディレクトリに移動したのち、以下を実行します。

.. code-block:: console

    $ rime_init --git

すると、``PROJECT`` というファイルと ``common/`` というフォルダ（および git 管理に用いられる ``.git/``, ``.gitignore``）が生成されます。``PROJECT`` にはいくつかの設定が書かれていますが、このチュートリアルではそれらを編集することはありません。

.. code-block:: console

    $ ls
    PROJECT    common


----

.. _quickstart-make_problem:


新規問題の作成
================================

それでは早速問題を作ってみましょう。今回は以下のような問題を準備することにします。

::

    問題
    整数 A, B が与えられます。 A + B を出力してください。

    制約
    0 <= A, B <= 10


まず、新規問題の作成を行います。以下のコマンドを実行してみましょう。

.. code-block:: console

    $ rime add . problem aplusb

上の ``aplusb`` のところが問題ディレクトリ名になります。例えば ``aminusb`` という問題ディレクトリを作りたければ、``rime add . problem aminusb`` とすれば良いです。詳しくはこちら [TODO] をご覧ください。


.. attention::

    上のコマンドを実行すると、コンソールが謎の文字列に覆われて再起不能に見えるようになるかもしれません。これは問題の設定ファイルをその場で編集できるようになっており、それに用いられる既定のエディタが Vim に設定されていることに起因するものです。もし Vim の使い方に明るくないならば、慌てずに ``:q`` と打ってエンターキーを押しましょう。


すると、``aplusb/`` というディレクトリが作られます。このディレクトリに移動してみましょう。中には、``PROBLEM`` というファイル 1 つだけが入っています。


.. code-block:: console

    $ ls
    PROJECT    aplusb     common
    $ cd aplusb/
    $ ls
    PROBLEM


``PROBLEM`` ファイルの中身は以下のようになっています。このファイルも、6 行目の ``time_limit`` （Rime におけるその問題の実行時間制限）の値を適宜変更する以外は基本的に編集する必要はありません。

.. code-block:: python
    :linenos:
    :caption: PROBLEM
    :emphasize-lines: 6

    # -*- coding: utf-8; mode: python -*-

    pid='X'

    problem(
      time_limit=1.0,
      id=pid,
      title=pid + ": Your Problem Name",
      #wiki_name="Your pukiwiki page name", # for wikify plugin
      #assignees=['Assignees', 'for', 'this', 'problem'], # for wikify plugin
      #need_custom_judge=True, # for wikify plugin
      #reference_solution='???',
    )

    atcoder_config(
      task_id=None # None means a spare
    )


.. tip::

    12 行目の ``reference_solution`` のコメントアウトを解除して適切に設定することで、想定解出力にどの解答プログラムを利用するかを選択することもできます。複数の解答プログラムで速度に差がある場合や、正答となる出力が複数存在する場合などに役立つかもしれません。

    詳しくは こちら [TODO] を参照してください。


----


.. _quickstart-make_solution:

解答プログラムの作成
================================

次に、解答プログラムを作成してみます。先ほど作成した ``aplusb/`` ディレクトリ内で、以下のコマンドを実行してみましょう。

.. code-block:: console

    $ rime add . solution cpp_correct

上の ``cpp_correct`` のところが解答プログラムのディレクトリ名になります。ここの名前はなんでも良いです。

すると、（エディタが起動したのち、） ``cpp_correct/`` というディレクトリが作られます。このディレクトリに移動してみましょう。中には、``SOLUTION`` というファイル 1 つだけが入っています。


.. code-block:: console

    $ ls
    PROBLEM     cpp_correct
    $ cd cpp_correct/
    $ ls
    SOLUTION


``SOLUTION`` ファイルの中身は以下のようになっています。

.. code-block:: python
    :linenos:
    :caption: SOLUTION
    :emphasize-lines: 5

    # -*- coding: utf-8; mode: python -*-

    ## Solution
    #c_solution(src='main.c') # -lm -O2 as default
    #cxx_solution(src='main.cc', flags=[]) # -std=c++11 -O2 as default
    #kotlin_solution(src='main.kt') # kotlin
    #java_solution(src='Main.java', encoding='UTF-8', mainclass='Main')
    #java_solution(src='Main.java', encoding='UTF-8', mainclass='Main',
    #              challenge_cases=[])
    #java_solution(src='Main.java', encoding='UTF-8', mainclass='Main',
    #              challenge_cases=['10_corner*.in'])
    #rust_solution(src='main.rs') # Rust (rustc)
    #go_solution(src='main.go') # Go
    #script_solution(src='main.sh') # shebang line is required
    #script_solution(src='main.pl') # shebang line is required
    #script_solution(src='main.py') # shebang line is required
    #script_solution(src='main.rb') # shebang line is required
    #js_solution(src='main.js') # javascript (nodejs)
    #hs_solution(src='main.hs') # haskell (stack + ghc)
    #cs_solution(src='main.cs') # C# (mono)

    ## Score
    #expected_score(100)


この中で、解答プログラムの言語に対応した行のコメントアウトを解除し、必要に応じてソースファイル名を変更します。今回は C++ の解答プログラムを追加するため、5 行目のコメントアウトを解除します。ファイル名については、今回は ``ans.cpp`` としてみます。


.. code-block:: python
    :linenos:
    :caption: SOLUTION（一部抜粋）
    :emphasize-lines: 1
    :lineno-start: 5

    cxx_solution(src='ans.cpp', flags=[]) # -std=c++11 -O2 as default


.. tip::

    実は、ここでコメントアウトを解除しなくとも、Rime はディレクトリ内のファイルの拡張子を参照することで解答プログラムの言語をよしなに解釈してくれます。ただ、想定誤解法の追加時にはここの設定が必須なので慣れておけると良いでしょう。


それでは、次は実際の解答プログラムを追加します。ここでは、``cpp_correct/`` ディレクトリ内に自分でファイルを作成してプログラムを書きます。この問題では、例えば以下のようなプログラムになるでしょう。


.. code-block:: cpp
    :linenos:
    :caption: ans.cpp

    #include <iostream>
    using namespace std;

    int main() {
        int a, b;
        cin >> a >> b;
        cout << a + b << '\n';
        return 0;
    }


----


テスト用プログラムの作成
================================

次に、テスト用プログラムを作成します。ここで言うテスト用プログラムとは、解答プログラムが正しく問題を解決するプログラムであるかどうかを判断するためのプログラムの総称であり、Rime においてユーザーが用意する必要のあるプログラムは主に以下の 2 つです。

入力生成器 (generator)
    解答プログラムに与える入力を生成するプログラム

入力検証器 (validator)
    解答プログラムに与える入力が問題の制約を正しく満たしているかを検証するプログラム


.. tip::

    想定される出力が複数ある場合や出力された実数の誤差を許容する場合などに、加えて **出力検証器 (judge)** が必要になることもあります。


それでは、テスト用プログラムを作成していきます。 ``aplusb/`` ディレクトリ内で、以下のコマンドを実行してみましょう。


.. code-block:: console

    $ rime add . testset tests


上の ``tests`` のところがテスト用プログラムのディレクトリ名になります。ここの名前はなんでも良いですが、慣例的に ``tests`` という名称が用いられることが多いです。

すると、（エディタが起動したのち、） ``tests/`` というディレクトリが作られます。このディレクトリに移動してみましょう。中には、``TESTSET`` というファイル 1 つだけが入っています。


.. code-block:: console

    $ ls
    PROBLEM     cpp_correct tests
    $ cd tests/
    $ ls
    TESTSET


``TESTSET`` ファイルの中身は以下のようになっています。


.. code-block:: python
    :linenos:
    :caption: TESTSET
    :emphasize-lines: 5,13

    # -*- coding: utf-8; mode: python -*-

    ## Input generators.
    #c_generator(src='generator.c')
    #cxx_generator(src='generator.cc', dependency=['testlib.h'])
    #java_generator(src='Generator.java', encoding='UTF-8', mainclass='Generator')
    #rust_generator(src='generator.rs')
    #go_generator(src='generator.go')
    #script_generator(src='generator.pl')

    ## Input validators.
    #c_validator(src='validator.c')
    #cxx_validator(src='validator.cc', dependency=['testlib.h'])
    #java_validator(src='Validator.java', encoding='UTF-8',
    #               mainclass='tmp/validator/Validator')
    #rust_validator(src='validator.rs')
    #go_validator(src='validator.go')
    #script_validator(src='validator.pl')

    ## Output judges.
    #c_judge(src='judge.c')
    #cxx_judge(src='judge.cc', dependency=['testlib.h'],
    #          variant=testlib_judge_runner)
    #java_judge(src='Judge.java', encoding='UTF-8', mainclass='Judge')
    #rust_judge(src='judge.rs')
    #go_judge(src='judge.go')
    #script_judge(src='judge.py')

    ## Reactives.
    #c_reactive(src='reactive.c')
    #cxx_reactive(src='reactive.cc', dependency=['testlib.h', 'reactive.hpp'],
    #             variant=kupc_reactive_runner)
    #java_reactive(src='Reactive.java', encoding='UTF-8', mainclass='Judge')
    #rust_reactive(src='reactive.rs')
    #go_reactive(src='reactive.go')
    #script_reactive(src='reactive.py')

    ## Extra Testsets.
    # icpc type
    #icpc_merger(input_terminator='0 0\n')
    # icpc wf ~2011
    #icpc_merger(input_terminator='0 0\n',
    #            output_replace=casenum_replace('Case 1', 'Case {0}'))
    #gcj_merger(output_replace=casenum_replace('Case 1', 'Case {0}'))
    id='X'
    #merged_testset(name=id + '_Merged', input_pattern='*.in')
    #subtask_testset(name='All', score=100, input_patterns=['*'])
    # precisely scored by judge program like Jiyukenkyu (KUPC 2013)
    #scoring_judge()


この中で、テスト用プログラムの言語に対応した行のコメントアウトを解除し、必要に応じてソースファイル名を変更します。今回は C++ の generator, validator を追加するため、それぞれ該当する行のコメントアウトを解除します。


.. code-block:: python
    :linenos:
    :caption: TESTSET（一部抜粋）
    :emphasize-lines: 1
    :lineno-start: 5

    cxx_generator(src='generator.cc', dependency=['testlib.h'])


.. code-block:: python
    :linenos:
    :emphasize-lines: 1
    :lineno-start: 13

    cxx_validator(src='validator.cc', dependency=['testlib.h'])


さて、それでは入力生成器と入力検証器を作成していきます。これらは、Rime では `testlib <https://github.com/MikeMirzayanov/testlib>`_ というライブラリを用いて書かれることが多いです。


``tests/`` ディレクトリ内に、 ``generator.cc`` と ``validator.cc`` を追加します。入力生成器、入力検証器の詳しい仕様については こちら [TODO] をご覧ください。


.. code-block:: cpp
    :linenos:
    :caption: generator.cc

    #include <iostream>
    #include "testlib.h"
    using namespace std;

    const int MIN_A = 1;
    const int MAX_A = 10;
    const int MIN_B = 1;
    const int MAX_B = 10;

    int main(int argc, char** argv) {
        registerGen(argc, argv, 1);
        for (int t = 0; t < 10; t++) {
            ofstream of(format("02_random_%02d.in", t + 1).c_str());
            int a = rnd.next(MIN_A, MAX_A);
            int b = rnd.next(MIN_B, MAX_B);
            of << a << ' ' << b << '\n';
            of.close();
        }
        return 0;
    }


.. code-block:: cpp
    :linenos:
    :caption: validator.cc

    #include <iostream>
    #include "testlib.h"
    using namespace std;

    const int MIN_A = 1;
    const int MAX_A = 10;
    const int MIN_B = 1;
    const int MAX_B = 10;

    int main(int argc, char** argv) {
        registerValidation(argc, argv);
        inf.readInt(MIN_A, MAX_A, "A");
        inf.readSpace();
        inf.readInt(MIN_B, MAX_B, "B");
        inf.readEoln();
        inf.readEof();
        return 0;
    }


上の入力生成器はランダムな入力を生成しますが、それ以外にサンプル入力やコーナーケースなど手で作ったケースを入れたくなるかもしれません。そういう場合は、 ``tests/`` ディレクトリ以下に ``.in`` という拡張子で入力ファイルを置いておくことで入力に含めることができます。


----

テストの実行
================================

ようやく準備が整ったので、テストを実行します。 ``aplusb/`` ディレクトリに戻り、以下のコマンドを実行してみましょう。


.. code-block:: console

    $ rime test
    [ COMPILE  ] aplusb/tests: generator.cc
    [ COMPILE  ] aplusb/tests: validator.cc
    [ GENERATE ] aplusb/tests: generator.cc
    [ VALIDATE ] aplusb/tests: OK
    [ COMPILE  ] aplusb/cpp_correct
    [  REFRUN  ] aplusb/cpp_correct
    [   TEST   ] aplusb/cpp_correct: max 0.00s, acc 0.03s

    Build Summary:
    aplusb ... in: 40B, diff: 25B, md5: -
      cpp_correct CXX 9 lines, 130B

    Test Summary:
    aplusb ... 1 solutions, 10 tests
      cpp_correct  OK  max 0.00s, acc 0.03s

    Error Summary:
    Total 0 errors, 0 warnings


.. attention::

    **謎のコンパイルエラーでテストができない場合**

    ひょっとしてあなたはいま Mac を使っていて、かつ ``bits/stdc++.h`` をインクルードしていませんか？ Rime では C++ のコンパイル時に環境変数 ``CXX`` を参照し、定義されていない場合は ``g++`` を使用します。Mac では ``g++`` と打つと clang が動くので ``bits/stdc++.h`` が無いと言われてしまいます。解決策としては ``bits/stdc++.h`` を使わないか、もしくは以下のように環境変数 ``CXX`` を指定してあげれば良いです（``g++-10`` のところは、必要に応じてインストールされている GCC のコマンド名に置き換えてください）。

    .. code-block:: console

        $ CXX=g++-10 rime test


無事にテストをすることができました。
