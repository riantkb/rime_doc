インストール
============


.. attention::

    ここでは、Git と Python（および pip）がすでにインストール済みであるとして説明をします。

    * もし Git がインストールされていない場合は `こちら <https://git-scm.com/book/ja/v2/%E4%BD%BF%E3%81%84%E5%A7%8B%E3%82%81%E3%82%8B-Git%E3%81%AE%E3%82%A4%E3%83%B3%E3%82%B9%E3%83%88%E3%83%BC%E3%83%AB>`__ などを参考にインストールをしてください。
    * もし Python がインストールされていない場合は `こちら <https://www.python.jp/install/install.html>`__ などを参考にインストールをしてください。



以下のコマンドをコマンドライン上で実行することでインストールができます（先頭の $ は入力待ちを示す記号ですので、入力する必要はありません）。

.. code-block:: console

    $ pip install git+https://github.com/icpc-jag/rime

インストールが正常に完了していると、``rime`` というコマンドと ``rime_init`` というコマンドが使えるようになっているはずです。

.. code-block:: console

    $ rime --help
    rime.py <command> [<options>...] [<args>...]

        Rime is a tool for programming contest organizers to automate usual, boring
        and error-prone process of problem set preparation. It supports various
        programming contest styles like ACM-ICPC, TopCoder, etc. by plugins.

        To see a brief description and available options of a command, try:

        rime.py help <command>

    Commands:

    build    Build a target and its dependencies.
    clean    Clean intermediate files.
    help     Show help.
    test     Run tests in a target.

    Global options:

    -C, --cache_tests  Cache test results.
    -d, --debug        Turn on debugging.
    -h, --help         Show this help.
    -j, --jobs <n>     Run multiple jobs in parallel.
    -k, --keep_going   Do not skip tests on failures.
    -p, --precise      Do not run timing tasks concurrently.
    -q, --quiet        Skip unimportant message.
