02_amend_commit_log 解答
========

### 問題2-1
--------------------
今、02_amend_commit_logのgitレポジトリはBeforeの状態である。  
しかし、commit.txtの最後に`5th commit`の文字列を追記し忘れてしまった。  

commit.txtに5th commitの文字列を追加し、直近のコミットに変更分を追加することで、  
02_amend_commit_logのgitレポジトリをAfter1の状態にせよ。
この時、コミットログコメントは`4th & 5th commmit`とする。 

### <a name="ans2-1">問題2−1 解答</a>
--------------------

```
➤  vim commit.txt (行末に5th commitを追記)
➤  git add commit.txt 
➤  git commit --amend
[master 0a4d0e9] 4th & 5th commmit
 Date: Mon Nov 3 10:19:17 2014 +0900
 1 file changed, 2 insertions(+)
```

### 問題2-2
--------------------
無事修正が終わったと思いきや、コミットログコメントにtypeがあった。  
このままだと格好が悪いので、`4&5th commmit` を`4&5th commit`に修正したい。
02_amend_commit_logのgitレポジトリをAfter1からAfter2の状態に修正せよ。

### <a name="ans2-2">問題2−2 解答</a>
--------------------
```
➤  git commit --amend
[master 895b5ec] 4th & 5th commit
 Date: Mon Nov 3 10:19:17 2014 +0900
 1 file changed, 2 insertions(+)
```