03_clean_up_commit : 過去のコミットをまとめて修正する、綺麗にまとめる
========

### 問題3-1
--------------------
今、03_clean_up_commitのgitレポジトリはBeforeの状態である。  
しかし、commit.txtの文字列は本来`XX line`にする予定だったが、誤って`XX commit`にしていることに後日気がついた。
03_clean_up_commitのgitレポジトリをAfter1の状態にして、当初の予定通りの文字列をcommit.txtに記載せよ。

### <a name="ans3-1">問題3−1 解答</a>
--------------------
```
➤  git rebase -i --root
        
＃(1st commit -> 1st lineに変更)
➤  vim commit.txt 
➤  git add commit.txt
➤  git commit --amend
➤  git rebase --continue

＃(2nd commit -> 2nd lineに変更&conflictの解消)
➤  vim commit.txt 
➤  git add commit.txt
➤  git rebase --continue

＃(3rd commit -> 3rd lineに変更&conflictの解消)
➤  vim commit.txt
➤  git add commit.txt
➤  git rebase --continue

＃(4,5th commit ->  4,5lineに変更&conflictの解消)
➤  vim commit.txt
➤  git add commit.txt
➤  git rebase --continue

➤  git log --oneline
d4e70a9 4&5th commit
f0be864 3rd commit
f20659e 2nd commit
7714ceb 1st commit

＃rebaseの結果確認
➤  cat commit.txt
1st line
2nd line
3rd line
4th line
5th line
```

### 問題3-2
--------------------
無事After1の状態にgitレポジトリを変更する事ができたが、まめにコミットをしすぎたためコミットログが汚くなってしまった。そこで、B',C',D'のコミットを1つにまとめることにした。03_clean_up_commitのgitレポジトリをAfter1からAfter2の状態にせよ。


### <a name="ans3-2">問題3−2 解答</a>
--------------------
```
＃綺麗にまとめる前のコミットログの確認
➤  git log --oneline
d4e70a9 4&5th commit
f0be864 3rd commit
f20659e 2nd commit
7714ceb 1st commit

＃HEADの位置の確認
➤  git graph
* d4e70a9  (HEAD, master) 2014-11-03 Takayuki WATANABE 4&5th commit
* f0be864  2014-11-03 Takayuki WATANABE 3rd commit
* f20659e  2014-11-03 Takayuki WATANABE 2nd commit
* 7714ceb  2014-11-03 Takayuki WATANABE 1st commit

➤  git rebase -i HEAD~~~
＃rebaseの内容
pick f20659e 2nd commit
s f0be864 3rd commit
s d4e70a9 4&5th commit

＃コミットがまとまっているか確認
➤  git log --oneline
9c7ae76 2nd&3rd&4&5th commit
7714ceb 1st commit

＃rebase結果の確認
➤  cat commit.txt
1st line
2nd line
3rd line
4th line
5th line

```

### Link
--------------------
 * Previous: [02_amend_commit_log : 直近のコミット、コミットログを修正する問題](02_amend_commit_log.md)
 * Next: [04_handle_branch : ブランチを使いこなす](04_handle_branch.md) 
 * 目次: [README](README.md)
 