インストール
============


.. attention::

    ここでは、Python（および pip）がすでにインストール済みであるとして説明をします。もしまだ Python をインストールしていない場合は、https://www.python.jp/install/install.html などを参考にインストールをしてください。


以下のコマンドをコマンドライン上で実行することでインストールができます（先頭の $ は入力待ちを示す記号です）。

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


.. admonition:: COMMENT

    ``rime init`` ではなく ``rime_init`` であることになにか意味があるのだろうか。
    あと、何も指定しなかった場合に ``--git`` がデフォルトになってほしい（git と同じように ``rime init`` とすると init できてほしい）気持ちも少しはある。
