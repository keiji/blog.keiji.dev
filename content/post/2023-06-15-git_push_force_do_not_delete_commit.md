---
title: git push --forceで履歴は消えないと言う話
author: Keiji Ariyama
type: post
date: 2023-06-15T15:45:00+09:00
url: /2023/06/git_push_force_do_not_delete_commit.html
categories:
  - 雑記

---

　GitのForce push（`git push --force`）と言えば、間違って実行してリポジトリ吹き飛ばしたとか、ミスオペレーションの代名詞として語られることが多い操作です。
筆者も過去に一度だけmasterブランチに向けてForce pushをしてしまい、平謝りをした経験があります。

　一方、最近知ったのですが、Gitリポジトリに本来入れてはいけない情報を入れたときの対応策としてForce pushすれば履歴は消えると考えている人もいるようです。

　これについては、言葉で説明するよりやってみた方が早そうなので、実際に試してた結果を共有します。
もしお近くで「Force pushで消える、いや消えない」と言った議論を聞いたときには、そっとこの記事と、次の記事を見せてあげてください。

GitHub上のsensitive dataを削除するための手順と道のり - Merpay Advent Calendar 2021
https://engineering.mercari.com/blog/entry/20211207-removing-sensitive-data-from-github/

　また、この記事について誤りや修正すべき点などがあれば、是非ご指摘いただければと思います。
最近はBlueskyにいます（Twitterはやっていません）。

https://staging.bsky.app/profile/keiji.bsky.social

<!--more-->

## 実験

実験は、次のステップで行いました。

 * リポジトリを作成・cloneする
 * 履歴を作成する（repo1）
 * 履歴を作成する（repo2）
 * リポジトリの状態を確認する（repo3）
 * 消えた履歴を復元する（repo3）

### リポジトリを作成・cloneする

　まず、ローカルにGitのリポジトリ`mygreatrepository`を作ります。

```
$ cd forcepushtest
$ git init --bare mygreatrepository
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint:
hint: 	git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint:
hint: 	git branch -m <name>
Initialized empty Git repository in /Users/keiji_ariyama/forcepushtest/mygreatrepository/
```

サーバー上のリポジトリを再現するためにオプション`--bare`を付けています。

```
$ ls mygreatrepository
HEAD		description	info		refs
config		hooks		objects
```

　次に、これをcloneします。今回は3つくらい作っておきましょう。

```
$ git clone mygreatrepository repo1
Cloning into 'repo1'...
warning: You appear to have cloned an empty repository.

$ git clone mygreatrepository repo2
Cloning into 'repo2'...
warning: You appear to have cloned an empty repository.

$ git clone mygreatrepository repo3
Cloning into 'repo3'...
warning: You appear to have cloned an empty repository.
```

　これで`mygreatrepository`を参照する3つのディレクトリができました。

### 履歴を作成する（repo1）

　早速`repo1`に移動して、ファイルをコミットしていきます。

```
$ cd repo1
$ touch README.md
$ git add README.md
$ git commit -m 'First commit'
[master (root-commit) 5429edc] First commit
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 README.md
$ ls
README.md
```

　さらに履歴を作ります。

```
$ echo "# My great repository" > README.md
$ git add README.md
$ git commit -m 'Update README.md'
 1 file changed, 1 insertion(+)
$ git log
commit f01a8e4beb6eba37a5ee451c9c589964cc85e6fd (HEAD -> master)
Author: ARIYAMA Keiji <keiji.ariyama@gmail.com>
Date:   Thu Jun 15 14:58:55 2023 +0900

    Update

commit 5429edccda1370e2c4b8e8b21f107ea72d26dbed
Author: ARIYAMA Keiji <keiji.ariyama@gmail.com>
Date:   Thu Jun 15 14:57:22 2023 +0900

    First commit
```

　この2つのコミットからなる履歴を、originのリポジトリ`mygreatrepository`にpushします。

```
$ git push origin master
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 10 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (6/6), 820 bytes | 820.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
To /Users/keiji_ariyama/forcepushtest/mygreatrepository
 * [new branch]      master -> master
```

　これでリポジトリのmasterは`f01a8e4beb6eba37a5ee451c9c589964cc85e6fd`で示されるコミットになりました。

### 履歴を作成する（repo2）

　続いて、同じリポジトリをcloneした別の場所`repo2`に移動して、まったく別の履歴を作ります。

```
$ cd ../repo2
$ git log
fatal: your current branch 'master' does not have any commits yet
$ touch HELLO_WORLD.md
$ git add HELLO_WORLD.md
$ git commit -m 'Create HELLO_WORLD.md'
[master (root-commit) 10bf7e9] Create HELLO_WORLD.md
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 HELLO_WORLD.md
$ git log
commit 10bf7e96a13e5b1f154499129a5149509e21e7f0 (HEAD -> master)
Author: ARIYAMA Keiji <keiji.ariyama@gmail.com>
Date:   Thu Jun 15 15:03:33 2023 +0900

    Create HELLO_WORLD.md
```

　`repo2`で新しく作った履歴をpushします。

```
$ git push origin master
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to '/Users/keiji_ariyama/forcepushtest/mygreatrepository'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

　このようにエラーになります。すでに`repo1`の履歴がpushされているからです。
そこでみんな大好きForce push（`git push --force`）を使います。

```
$ git push -f origin master
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 411 bytes | 411.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To /Users/keiji_ariyama/forcepushtest/mygreatrepository
 + f01a8e4...10bf7e9 master -> master (forced update)
```

これで、リポジトリ`mygreaterepository`の履歴は強制的に書き換わり、masterの先頭は`10bf7e96a13e5b1f154499129a5149509e21e7f0`で示されるコミットになりました。

### リポジトリの状態を確認する（repo3）

　リポジトリの状態を確認するため、`repo3`に移動して`pull`します。

```
$ ../repo3
$ git pull origin master
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 391 bytes | 391.00 KiB/s, done.
From /Users/keiji_ariyama/forcepushtest/mygreatrepository
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> origin/master
$ ls
HELLO_WORLD.md
$ git log
commit 10bf7e96a13e5b1f154499129a5149509e21e7f0 (HEAD -> master, origin/master)
Author: ARIYAMA Keiji <keiji.ariyama@gmail.com>
Date:   Thu Jun 15 15:03:33 2023 +0900

    Create HELLO_WORLD.md
```

　期待した状態になっていることが確認できました。

### 消えた履歴を復元する（repo3）

　さて、この状態だとrepo1に追加した2つのコミット `f01a8e4beb6eba37a5ee451c9c589964cc85e6fd`と`5429edccda1370e2c4b8e8b21f107ea72d26dbed`からなる履歴は消えたように見えます。
しかし、データとしてはしっかり残っていて、コミットハッシュを指定すれば手元にpullして履歴を復元できます。

```
$ git pull origin f01a8e4beb6eba37a5ee451c9c589964cc85e6fd
remote: Enumerating objects: 6, done.
remote: Counting objects: 100% (6/6), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (6/6), 800 bytes | 400.00 KiB/s, done.
From /Users/keiji_ariyama/forcepushtest/mygreatrepository
 * branch            f01a8e4beb6eba37a5ee451c9c589964cc85e6fd -> FETCH_HEAD
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint:
hint:   git config pull.rebase false  # merge
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint:
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
fatal: Need to specify how to reconcile divergent branches.
$ git checkout f01a8e4beb6eba37a5ee451c9c589964cc85e6fd -b recovered
Switched to a new branch 'recovered'
$ git log
commit f01a8e4beb6eba37a5ee451c9c589964cc85e6fd (HEAD -> recovered)
Author: ARIYAMA Keiji <keiji.ariyama@gmail.com>
Date:   Thu Jun 15 14:58:55 2023 +0900

    Update

commit 5429edccda1370e2c4b8e8b21f107ea72d26dbed
Author: ARIYAMA Keiji <keiji.ariyama@gmail.com>
Date:   Thu Jun 15 14:57:22 2023 +0900

    First commit
```

## 実験結果

　以上のことから、リポジトリに新しい履歴をForce pushをしても、コミットのデータは消えずに残っている。Force pushするだけではGitの履歴は消えないことが分かりました。

　機微なデータを含むコミットを書き換えて、履歴を再構築したものをForce pushすれば、一見消えたように見えますが、内部的にはデータとしては残っていることになります。

　「コミットハッシュを知られなければ良いだろう」という話もあるかもしれませんが、GitHubだとPull Requestなどそこかしこにハッシュが書いてあります。また、いずれかのコミットメッセージに参考として書いてある可能性もあります。
　コミットハッシュが知られなければ良いと言う意見を、ぼくは個人的には支持できませんが、そこは、機微データの重要度との兼ね合いで判断していただければと思います。

## 履歴の消し方

### Gitコマンドでリポジトリに直接アクセスできる場合

　今回のケースでは、リモートリポジトリのディレクトリに直接アクセスできるので、`git gc --prune=now`することで履歴を消すことができました。

```
$ cd ../mygreatrepository
$ git gc --prune=now
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), done.
Building bitmaps: 100% (1/1), done.
Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
$ cd ../repo2
$ git pull origin f01a8e4beb6eba37a5ee451c9c589964cc85e6fd
fatal: git upload-pack: not our ref f01a8e4beb6eba37a5ee451c9c589964cc85e6fd
fatal: remote error: upload-pack: not our ref f01a8e4beb6eba37a5ee451c9c589964cc85e6fd
```

### GitHub等の外部サービスの場合

　GitHubやBitbucketと言ったサービスに組み込まれたGitは`git gc`などが使えないので、サポートに連絡を取って対応を求める必要があるという認識です。
詳細はお使いのサービスのマニュアルやサポートにお問い合わせください。

　また再掲になりますが、それについては次の記事に詳しく書いてあるので参考にしてください（これを見ながら、ぼくも手続きをしたことがありますが、情報が消えるまではずっと冷や汗を流していました）。

GitHub上のsensitive dataを削除するための手順と道のり - Merpay Advent Calendar 2021
https://engineering.mercari.com/blog/entry/20211207-removing-sensitive-data-from-github/


----

　最後になりましたが、この記事の内容を必要とする人が今後生まれないことを切実に願っています。
