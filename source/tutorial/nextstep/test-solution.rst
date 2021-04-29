誤答が落ちることをテストする
================================================================

:doc:`../quickstart` では「正しいプログラムが正しく受理されること」をテストしましたが、これで本当に十分でしょうか。実際には、「正しくないプログラムが正しく落ちること」も同じだけ重要なので、このことについてもテストを行うべきです。

この項では、 :doc:`../quickstart` の :ref:`quickstart-make_problem` で作成した A + B の問題を用いて誤答のテストの例を示します。


----


まず、 :doc:`../quickstart` の :ref:`quickstart-make_solution` と同様に解答プログラムを作成します。 ``aplusb/`` ディレクトリ内で、以下のコマンドを実行しましょう。

.. code-block:: console

    $ rime add . solution cpp_wa


次に、生成された ``SOLUTION`` ファイルを編集します。解答プログラムの言語に対応した行のコメントアウトを解除し、必要に応じてソースファイル名を変更するところまでは :doc:`../quickstart` と同様です。今回は C++ の解答プログラムを追加するため、5 行目のコメントアウトを解除します。ファイル名については、 ``ans.cpp`` としてみます。

ここで、 ``cxx_solution`` の引数に ``challenge_cases=[]`` を追加します。このようにすることで、「このプログラムは落ちなければならない」ということを指定することができます。


.. code-block:: python
    :linenos:
    :caption: SOLUTION（一部抜粋）
    :emphasize-lines: 1
    :lineno-start: 5

    cxx_solution(src='ans.cpp', flags=[], challenge_cases=[]) # -std=c++11 -O2 as default


.. tip::

    ``challenge_cases`` は、正確には ``challenge_cases=['10_corner_01.in']`` などというように入力ファイルをいくつか指定し、そのうち 1 つ以上で正答を返さないことをテスト通過の条件とするものです。入力ファイルを 1 つも指定しない場合、指定された入力ファイルが入力ファイル全体となるため、ほとんどの場合は 1 つも指定しない使い方で事足りるでしょう。


それでは、実際に ``ans.cpp`` を変更して正しい答えを返さないプログラムを用意してみましょう。今回は、答えが偶数のときに 1 大きい答えを出力するように変更してみます。


.. code-block:: cpp
    :linenos:
    :caption: ans.cpp

    #include<iostream>
    using namespace std;

    int main() {
        int a, b;
        cin >> a >> b;
        int ans = a + b;
        if (ans % 2 == 0) {
            ans++;
        }
        cout << ans << '\n';
        return 0;
    }


それでは ``aplusb/`` ディレクトリに戻り、テストを実行してみましょう。

.. code-block:: console
    :emphasize-lines: 20

    $ rime test
    [ COMPILE  ] aplusb/tests: generator.cc
    [ COMPILE  ] aplusb/tests: validator.cc
    [ GENERATE ] aplusb/tests: generator.cc
    [ VALIDATE ] aplusb/tests: OK
    [ COMPILE  ] aplusb/cpp_correct
    [  REFRUN  ] aplusb/cpp_correct
    [   TEST   ] aplusb/cpp_correct: max 0.01s, acc 0.05s
    [ COMPILE  ] aplusb/cpp_wa
    [   TEST   ] aplusb/cpp_wa: 02_random_04.in: Wrong Answer

    Build Summary:
    aplusb ... in: 40B, diff: 25B, md5: -
      cpp_correct CXX  9 lines, 130B
      cpp_wa      CXX 13 lines, 194B

    Test Summary:
    aplusb ... 2 solutions, 10 tests
      cpp_correct  OK  max 0.01s, acc 0.05s
      cpp_wa       OK  02_random_04.in: Wrong Answer

    Error Summary:
    Total 0 errors, 0 warnings


``Test Summary`` の ``cpp_wa`` の部分を見ると、 ``02_random_04.in: Wrong Answer`` と書いてあり、 ``02_random_04.in`` というケースで間違った答えを出力したのでテストは成功した、ということがわかります。


----


次に、誤答プログラムを少し変更してみましょう。今度は答えが 20 のときに RE となる（終了ステータスが 0 でない）ように変更してみます。


.. code-block:: cpp
    :linenos:
    :caption: ans.cpp

    #include<iostream>
    using namespace std;

    int main() {
        int a, b;
        cin >> a >> b;
        int ans = a + b;
        if (ans == 20) {
            return 1;
        }
        cout << ans << '\n';
        return 0;
    }


テストを実行してみます。

.. code-block:: console
    :emphasize-lines: 14

    $ rime test
    [   TEST   ] aplusb/cpp_correct: max 0.01s, acc 0.04s
    ERROR: aplusb/cpp_wa: Unexpectedly accepted all test cases
    [   TEST   ] aplusb/cpp_wa: Unexpectedly accepted all test cases

    Build Summary:
    aplusb ... in: 40B, diff: 25B, md5: -
      cpp_correct CXX  9 lines, 130B
      cpp_wa      CXX 13 lines, 194B

    Test Summary:
    aplusb ... 2 solutions, 10 tests
      cpp_correct  OK  max 0.01s, acc 0.04s
      cpp_wa      FAIL Unexpectedly accepted all test cases

    Error Summary:
    ERROR: aplusb/cpp_wa: Unexpectedly accepted all test cases
    Total 1 errors, 0 warnings


生成したランダムケースには A = B = 10 のケースは入っていなかったらしく、全てのテストケースで正答を返してしまいました。


----


それでは、このプログラムを落とせるケースを追加してみましょう。そのような入力を生成する入力生成器を追加しても良いのですが、今回は手で作ったケースを追加することで実現してみます。

``aplusb/tests/`` ディレクトリの中に ``05_corner_01.in`` というファイルを追加し、A = B = 10 のケースを用意します。


.. code-block::
    :caption: 05_corner_01.in

    10 10


この状態で、再びテストを行ってみます。


.. code-block:: console
    :emphasize-lines: 18

    $ rime test
    [ COMPILE  ] aplusb/tests: generator.cc
    [ COMPILE  ] aplusb/tests: validator.cc
    [ GENERATE ] aplusb/tests: generator.cc
    [ VALIDATE ] aplusb/tests: OK
    [  REFRUN  ] aplusb/cpp_correct
    [   TEST   ] aplusb/cpp_correct: max 0.01s, acc 0.06s
    [   TEST   ] aplusb/cpp_wa: 05_corner_01.in: Runtime Error

    Build Summary:
    aplusb ... in: 46B, diff: 28B, md5: -
      cpp_correct CXX  9 lines, 130B
      cpp_wa      CXX 13 lines, 194B

    Test Summary:
    aplusb ... 2 solutions, 11 tests
      cpp_correct  OK  max 0.01s, acc 0.06s
      cpp_wa       OK  05_corner_01.in: Runtime Error

    Error Summary:
    Total 0 errors, 0 warnings


きちんと RE が出て、テストが成功しました。


.. tip::

    ``challenge_cases`` で誤答であるということを指定する以外に、WA, RE, TLE などのうちどの判定になってほしいか、という期待する判定を指定することもできます。

    詳しくは こちら (TODO) を参照してください。
