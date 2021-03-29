設定ファイル
============

すべての設定ファイルは Python スクリプトとして記述され、Rime の起動時に eval されます。各設定ファイルの種類に応じていくつかの関数が Rime よりエクスポートされており、それらを呼ぶことにより設定を行います。

記述にあたっては Python の文法や標準ライブラリのすべてが利用可能ですが、可読性のためにシンプルな内容に留めておくことをおすすめします。一般的な用途では、設定関数の呼び出しを列挙するだけで十分です。

PROJECT
-------

プロジェクトディレクトリのトップレベルに置かれ、プロジェクト全体に共通の設定を記述します。

.. function:: use_plugin(name)

   このプロジェクトで用いるプラグインをロードします。
.. 詳しくは :ref:`plugins` を参照して下さい。

   例::

       use_plugin('merged_test')


PROBLEM
-------

問題ディレクトリの直下に置かれ、問題に関する設定を記述します。デフォルトでは :func:`problem` 関数のみが定義されており、 :func:`problem` 関数をちょうど一回呼び出さなければなりません。


.. function:: problem(time_limit, reference_solution=None, title=None, id=None)

   必要な引数はただひとつ *time_limit* のみです。この引数には、この問題に対する解答のテストをする際に、一つのテストケースに対して許される実行時間の上限を秒単位で指定します。

   *reference_solution* には、解答のテストを行う際に基準となる解答のディレクトリ名を指定します。基準解答はその出力が正しいと仮定され、ある問題に対する複数の解答がすべて一致していることを確認するときに、他の解答と出力を照合するために用いられます。指定がない場合は任意の解答がひとつ選ばれます。

   *title* には問題のタイトルを任意の文字列で指定します。

   *id* には問題のソートに使われる任意の文字列を指定します(一般的には 'A', 'B', ... が用いられます)。指定がない場合は問題ディレクトリ名でソートされます。

   例::

       problem(time_limit=3.0)
       problem(time_limit=3.0, reference_solution='solution1', id='C')


.. _configs-TESTSET:

TESTSET
-------

テストデータディレクトリに置かれ、問題の解答を検証するためのテスト入出力に関する設定を行います。

このファイルでは (1) 入力生成器, (2) 入力検証器, (3) 出力検証器 の 3 つが指定でき、それぞれ :func:`LANG_generator()`, :func:`LANG_validator()`, :func:`LANG_judge()` 関数を呼び出して登録します。各プログラムの仕様については :ref:`testspec` を参照してください。

.. function:: c_generator(src_name, flags=['-lm'])

   C で書かれた入力生成器を登録します。

   *src_name* にはソースコードの名前を拡張子付きで指定します。

   *flags* にはコンパイラに渡すフラグを文字列のリストで指定します。

   例::

       c_generator('generator.c')
       c_generator('generator.c', flags=['-lgmp'])

.. function:: cc_generator(src_name, flags=[])
              cxx_generator(src_name, flags=[])

   C++ で書かれた入力生成器を登録します。

   *src_name* にはソースコードの名前を拡張子付きで指定します。

   *flags* にはコンパイラに渡すフラグを文字列のリストで指定します。

   例::

       cc_generator('generator.cc')
       cxx_generator('generator.cpp', flags=['-lgmpxx'])

.. function:: java_generator(src_name, compile_flags=[], run_flags=[], encoding='UTF-8', mainclass='Main')

   Java で書かれた入力生成器を登録します。

   *src_name* にはソースコードの名前を拡張子付きで指定します。

   *compile_flags*, *run_flags* にはコンパイル時および実行時に渡すフラグを文字列のリストで指定します。

   *encoding* にはソースコードのエンコーディングを指定します。省略すると UTF-8 と見なされます。

   *mainclass* には実行する際のエントリポイントとなるクラス名を指定します。省略すると Main が使われます。

   例::

       java_generator('Generator.java')
       java_generator('Generator.java', compile_flags=['-source', '1.4'], run_flags=['-agentlib:hprof'], encoding='cp932', mainclass='Start')

.. function:: rust_generator(src_name, flags=[])

   Rust で書かれた入力生成器を登録します。

   *src_name* にはソースコードの名前を拡張子付きで指定します。

   *flags* にはコンパイラに渡すフラグを文字列のリストで指定します。

   例::

       rust_generator('generator.rs')
       rust_generator('generator.rs', flags=['-O'])

.. function:: script_generator(src_name, run_flags=[])

   スクリプト言語で書かれた入力生成器を登録します。スクリプトの冒頭は、使用するプログラミング言語を明示するために ``#!`` (shebang) で始まらなければなりません。Rime は shebang を解釈するためにスクリプトを perl インタプリタに渡すので、shebang が存在しない場合は perl スクリプトとして実行されます。

   *src_name* にはソースコードの名前を拡張子付きで指定します。

   *run_flags* には実行時にスクリプトに渡すパラメータを文字列のリストで指定します。

   例::

       script_generator('generator.rb')
       script_generator('generator.py', run_flags=['-R'])

.. function:: LANG_validator(src_name, **code_kwargs)

   入力検証器を登録します。言語と各種パラメータの指定方法は入力生成器と同じです。

   例::

       script_validator('validate.py')
       cc_validator('validator.cc', flags=['-lgmpxx'])

.. function:: LANG_judge(src_name, **code_kwargs)

   出力検証器を登録します。言語と各種パラメータの指定方法は入力生成器と同じです。

   例::

       script_judge('judge.py')
       cc_judge('judge.cc', flags=['-lgmpxx'])


SOLUTION
--------

解答ディレクトリに置かれ、解答プログラムに関する設定を行います。この中では :func:`LANG_solution` 関数をちょうど一回呼び出さなければなりません。

.. function:: LANG_solution(src_name, challenge_cases=None, **code_kwargs)

   解答プログラムを登録します。言語と各種パラメータの指定方法は入力生成器と同じですが、追加で指定できるパラメータが存在します。

   *challenge_cases* にはテスト入力ファイルの名前を列挙したリストを指定することができます。指定があった場合、この解答は誤答であるとマークされます。通常の解答はテストがすべて成功することをチェックされるのに対し、誤答はテストが失敗することがテストされます。また、誤答が基準解答として選ばれることはありません。 *challenge_cases* が空のリストである場合は全テスト中最低でも一つのテストに失敗することがチェックされ、空でないリストを指定した場合、指定されたテストのすべてに失敗することがチェックされます。

   例::

       script_solution('adhoc.py', run_flags=['-R'])
       cc_solution('wrong_any.cc', challenge_cases=[])
       cc_solution('wrong_some.cc', challenge_cases=['input1.in', 'input3.in'])
