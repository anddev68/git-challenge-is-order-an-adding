# 模範解答

まず、解答ブランチである `master` へ移動し、`users4.csv` をダウンロードします。

```sh-session
$ git checkout master
$ curl -O https://gist.githubusercontent.com/kikuchy/7d0a52299707005ecbd3/raw/3ac45a2b7d1fa7ad725ca2c9d5d84d9f45b11faa/users4.csv
```

では、`users4.csv` をリポジトリに追加しましょう。
おや、エラーメッセージが表示されるようです:

```sh-session
$ git add users4.csv
The following paths are ignored by one of your .gitignore files:
users4.csv
Use -f if you really want to add them.
```

エラーメッセージによると、このファイルは `.gitignore` というファイルによって無視する設定になっているようです。
`.gitignore` は、git の履歴で管理しないファイルを指定するファイルです。
では、`.gitignore` の中身を見てみましょう。

```sh-session
$ cat .gitignore
*
```

なんということでしょう、`*`（すべてを無視する）という驚愕の設定が書かれています。
かなり異常な設定といえそうですが、どのような意図をもって設定されたものなのかわかりません。
`git log` コマンドを使って、`.gitignore` が追加/編集されたときのコミットメッセージを確認しておきましょう。

```sh-session
$ git log --oneline .gitignore
925bb16 gitignoreっていうのがあると良いって聞いた
```

まったく有用なコミットメッセージではありませんね。
なぜ、このようなコミットがされたのかわかりません。
修正してよいかどうかは編集者本人に問い合わせるしかなさそうです。

今回は `.gitignore` を直すのは諦めることにします。
そして、`.gitignore` で無視されているファイルを強制的に追加するために `git add --force` コマンドを使います。

```sh-session
$ git add --force users4.csv
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   users4.csv
``` 

無事追加できたようです。
commit して push すれば正解となります。

```sh-session
$ git commit -m "Add users4"
$ git push origin master
```


# 教訓

1. `.gitignore` は慎重に設定しましょう。

    `.gitignore` の設定例は、[GitHub の公式リポジトリ](https://github.com/github/gitignore)に言語・OS・開発環境ごとに網羅されています。
    こちらを参考にするとよいでしょう。

    または、[gitignore.io](https://www.gitignore.io/) というサービスで自動生成してしまいましょう。

2. なぜ、その差分をいれたのかがわかるようにコミットメッセージを設定しましょう

    でないと、変更した本人に問い合わせるしかなくなってしまいます。
