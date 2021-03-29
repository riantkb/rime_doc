インストール
============


.. admonition:: TODO

    write requirements (and python version)


ここでは、Python がすでにインストール済みであるとして説明をします。https://github.com/icpc-jag/rime にあるように、以下のコマンドをシェルスクリプトで実行することでインストールができます。

.. code-block::

    pip install git+https://github.com/icpc-jag/rime

インストールが正常に完了していると、``rime`` というコマンドと ``rime_init`` というコマンドが使えるようになっているはずです。

.. admonition:: COMMENT

    ``rime init`` ではなく ``rime_init`` であることになにか意味があるのだろうか。
    あと、何も指定しなかった場合に ``--git`` がデフォルトになってほしい（git と同じように ``rime init`` とすると init できてほしい）気持ちも少しはある。
