# localsubmodtest_main
For a test of git's behaviour about submodule that is in the local sub directory

ローカルのサブディレクトリをGitローカルリポジトリにし，これをsubmoduleとして登録できるかの確認

```code: bash
git submodule add ./sub_neko
```

上はサブモジュールにできたが，mainディレクトリの方をリモートリポジトリに上げると，リモート上ではサブモジュールがあることだけしかわからず，ファイルもリポジトリへのリンクもない状態 (２つ目のコミット）

そこでサブモジュールを[リモート](https://github.com/Philosophistoria/localsubmodtest_sub)に上げる
そのうえで，`.gitmodule` 内のサブモジュールのurlをローカルからリモートに変更する

```diff
 [submodule "sub_neko"]
	path = sub_neko
-	url = ./sub_neko/
+	url = git@github.com:Philosophistoria/localsubmodtest_sub.git
```

さらにこの変更を`.git/config`にも伝える
```bash
git submodule sync
```

その後コミットすれば，mainのリモートリポジトリはsubのリモートリポジトリへの参照をもってサブモジュールを管理できる（[三番目のコミット](62c9a3d328ad66bcf2bd3a5e01c2150ff80173ec)）

サブモジュール側に更新があった場合（自分でいじった場合やsubmoduleのディレクトリに潜って`pull`した場合），メイン側でも`git status`に通知され，`add`して`commit`するか聞かれる．
ここでメインディレクトリで`git submodule update`をするとサブモジュールは一時的なブランチに分かれてそこに古いバージョンをロードする．

全く新しくサブモジュールを含んだリポジトリをクローンするときは`--recursive`オプションを付けるか，

```bash
git clone ---recursive git@github.com:Philosophistoria/localsubmodtest_main.git
```

あるいは `submodule init` `submodule update`をする必要がある
```bash
git clone git@github.com:Philosophistoria/localsubmodtest_main.git
git submodule init
git submodule update
```
