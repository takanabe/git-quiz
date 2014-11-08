04_handle_branch : ブランチを使いこなす
========

### 問題4-1
--------------------
#### シナリオ
あるサービスの新機能の開発のために、masterブランチからfunction１ブランチを切り、
function.txtにfunc1を追記することになった。
#### 問題
04_handle_branchのgitレポジトリの状態をBeforeからAfter1に変更せよ。  
  
### <a name="ans4-1">問題4−1 解答</a>
--------------------

```
➤  git checkout -b function1
＃function.txtの作成とfunc1の追記
➤  vim function.txt 
➤  git add function.txt
➤  git commit function.txt
＃ブランチとHEADの確認
➤  git graph
* b43e27d  (HEAD, function1) 2014-11-04 Takayuki WATANABE Add func
* 9c7ae76  (master) 2014-11-03 Takayuki WATANABE 2nd&3rd&4&5th commit
* 7714ceb  2014-11-03 Takayuki WATANABE 1st commit
```


### 問題4-2
--------------------
#### シナリオ
function1での新機能の開発が完了したため、masterブランチに変更を反映させたい。
04_handle_branchのgitレポジトリの状態をAfter1からAfter2に変更せよ。
また、After2のコミットログからだと他の開発メンバーがmasterブランチにfunction1ブランチを取り込んだのを判断しにくいと考えたため、After3の状態にすることにした。
#### 問題
After2の状態からコミットを遡り、After3の状態に変更せよ。
また、After3の状態になったことを確認したら、必要なくなったfunction1ブランチは削除すること。


### <a name="ans4-2">問題4−2 解答</a>
--------------------
fast-forware mergeとno fast-forward mergeの違いをきちんと理解しているか確認する問題  
まずはAfter2の状態に変更する。

```
➤  git graph
* b43e27d  (HEAD, function1) 2014-11-04 Takayuki WATANABE Add func
* 9c7ae76  (master) 2014-11-03 Takayuki WATANABE 2nd&3rd&4&5th commit
* 7714ceb  2014-11-03 Takayuki WATANABE 1st commit
➤  git checkout master
➤  git merge function1
＃ fast-forward mergeになっているか確認
➤  git graph
* b43e27d  (HEAD, master, function1) 2014-11-04 Takayuki WATANABE Add func
* 9c7ae76  2014-11-03 Takayuki WATANABE 2nd&3rd&4&5th commit
* 7714ceb  2014-11-03 Takayuki WATANABE 1st commit
```
次に、After2から遡りAfter3の状態にする。

```
#HEADの動きを調べる
➤  git reflog
b43e27d HEAD@{0}: merge function1: Fast-forward
9c7ae76 HEAD@{1}: checkout: moving from function1 to master
b43e27d HEAD@{2}: commit: Add func
9c7ae76 HEAD@{3}: checkout: moving from master to function1
9c7ae76 HEAD@{4}: clone: from git@github.com:takanabe/git-quiz_04_handle_branch.git
＃function1からmasterにチェックアウトした状態に戻す
➤  git reset --hard HEAD@{1}
＃無事に戻れたか確認する
➤  git graph
* b43e27d  (function1) 2014-11-04 Takayuki WATANABE Add func
* 9c7ae76  (HEAD, master) 2014-11-03 Takayuki WATANABE 2nd&3rd&4&5th commit
* 7714ceb  2014-11-03 Takayuki WATANABE 1st commit
➤  git merge --no-ff function1
```

03_clean_up_commitでは`git log`でHEADの位置を確認した後、`git reset --hard HEAD~~~`という方法で過去のコミットまでHEADを移動させているが、`git reflog`を利用したほうが直感的にわかりやすいのでおすすめ。



### 問題4-3 
--------------------
#### シナリオ
新機能function2の開発を行うことになった。そこで、function2のブランチを切り`function.txt`で`func2`の開発を始めた。しかし、しばらくすると、function2より新機能function3の開発を先に完了し、サービスに統合して欲しいとの依頼が来た。
#### 問題
そこで、function3のブランチを切り、それぞれ`function.txt`に`func3`を追加することにした。
04_handle_branchのgitレポジトリの状態をAfter3からAfter4に変更せよ。

### <a name="ans4-3">問題4−3 解答</a>
--------------------
```
➤  git checkout -b function2
＃func2の追記
➤  vim function.txt
➤  git add function.txt
➤  git commit
# masterブランチを元に、function3ブランチを生成してチェックアウト
➤  git checkout -b function3 master
＃func3の追記
➤  vim function.txt
➤  git add function.txt
➤  git add
➤  git commit
```


### 問題4-4 
--------------------
#### シナリオ
無事にfunction3の開発を完了したので、masterブランチにfunction3ブランチをマージした。その後、function2の開発も完了したので、masterブランチにマージしようとしたところ直前にマージしたfunction3とfunction.txt上でコンフリクトした。吟味した結果、function.txtではfunction2を先に記述するべきだと判断したので、func2,func3の順でfunction.txtに記載することでコンフリクトを解消することにした。
#### 問題
04_handle_branchのgitレポジトリの状態をAfter4からAfter5に変更せよ。 

### <a name="ans4-4">問題4−4 解答</a>
--------------------
```
➤  git checkout master
➤  git merge --no-ff function2
➤  git merge --no-ff function3
Auto-merging function.txt
CONFLICT (content): Merge conflict in function.txt
Automatic merge failed; fix conflicts and then commit the result.
＃コンフリクトを解消
➤  cat function.txt
func1
<<<<<<< HEAD
fucn2
=======
func3
>>>>>>> function3
＃コンフリクトを解消
➤  vim function.txt
➤  git add function.txt
➤  git commit
➤  git graph
*   cae22ff  (HEAD, master) 2014-11-04 Takayuki WATANABE Merge branch 'function3'
|\
* \   4234c3e  2014-11-04 Takayuki WATANABE Merge branch 'function2'
|\ \
| | * a6e4dda  (function3) 2014-11-04 Takayuki WATANABE Add func2
| |/
|/|
| * d8f642b  (function2) 2014-11-04 Takayuki WATANABE Add func2
|/
*   bfde948  2014-11-04 Takayuki WATANABE Merge branch 'function2'
|\
| * b43e27d  2014-11-04 Takayuki WATANABE Add func
|/
* 9c7ae76  2014-11-03 Takayuki WATANABE 2nd&3rd&4&5th commit
* 7714ceb  2014-11-03 Takayuki WATANABE 1st commit
＃function.txtの確認
➤  cat function.txt
func1
fucn2
func3
```
### 問題4-5 
--------------------
#### シナリオ
次にfunction4,function5を開発することになった。fucntion4はfunction5で利用する機能なのでfunction5ブランチを切りそこで両方の開発を進めることにした。function5ブランチでfunction4の開発が終わり、function5の開発をしていると、異なる機能function6を開発して今すぐmasterブランチに統合して欲しいという依頼が来た。そこで、function5の作業内容を一時的にコミットして、funtion6ブランチを切りfunction6を開発することにした。しかし、function6もfunction4が必要であることがわかった。そこで、function5ブランチで作成したfunction4のコミットをfucntion6ブランチに持ってくることにした。
#### 問題
04_handle_branchのgitレポジトリの状態をAfter5からAfter6に変更せよ。

### <a name="ans4-5">問題4−5 解答</a>
まずは、function5の一時的なコミットまでを実施する。

```
➤  git checkout -b function5
➤  git graph
*   7438105  (HEAD, master, function5) 2014-11-08 Takayuki WATANABE Merge branch 'function2'
|\
* \   13713c4  2014-11-08 Takayuki WATANABE Merge branch 'function3'
|\ \
| * | a119fb6  (function3) 2014-11-08 Takayuki WATANABE Add function3
|/ /
| * ca3afef  (function2) 2014-11-08 Takayuki WATANABE Add func2
|/
*   04b7397  2014-11-08 Takayuki WATANABE Merge branch 'function1'
|\
| * a26701d  (function1) 2014-11-08 Takayuki WATANABE Add func1
|/
* 9c7ae76  2014-11-03 Takayuki WATANABE 2nd&3rd&4&5th commit
* 7714ceb  2014-11-03 Takayuki WATANABE 1st commit
➤  cat function.txt
func1
func2
func3
＃ func4の追加
➤  vim function.txt
➤  git add function.txt
➤  git commit
[function5 b94036a] Add func4
# funの追加
➤  vim function.txt
➤  git add function.txt
＃　一時的にコミット
➤  git commit
[function5 a3fa652] Add temporary func5 commit
```

次にfunction6の開発を行う。

```
➤  git log --oneline
a3fa652 Add temporary func5 commit
b94036a Add func4
7438105 Merge branch 'function2'
13713c4 Merge branch 'function3'
a119fb6 Add function3
ca3afef Add func2
04b7397 Merge branch 'function1'
a26701d Add func1
9c7ae76 2nd&3rd&4&5th commit
7714ceb 1st commit
➤  git checkout -b function6 master
➤  git log --oneline
7438105 Merge branch 'function2'
13713c4 Merge branch 'function3'
a119fb6 Add function3
ca3afef Add func2
04b7397 Merge branch 'function1'
a26701d Add func1
9c7ae76 2nd&3rd&4&5th commit
7714ceb 1st commit
➤  git cherry-pick b94036a
[function6 63b305d] Add func4
 Date: Sat Nov 8 09:56:50 2014 +0900
 1 file changed, 1 insertion(+)
➤  git graph
* 63b305d  (HEAD, function6) 2014-11-08 Takayuki WATANABE Add func4
| * a3fa652  (function5) 2014-11-08 Takayuki WATANABE Add temporary func5 commit
| * b94036a  2014-11-08 Takayuki WATANABE Add func4
|/
*   7438105  (master) 2014-11-08 Takayuki WATANABE Merge branch 'function2'
|\
* \   13713c4  2014-11-08 Takayuki WATANABE Merge branch 'function3'
|\ \
| * | a119fb6  (function3) 2014-11-08 Takayuki WATANABE Add function3
|/ /
| * ca3afef  (function2) 2014-11-08 Takayuki WATANABE Add func2
|/
*   04b7397  2014-11-08 Takayuki WATANABE Merge branch 'function1'
|\
| * a26701d  (function1) 2014-11-08 Takayuki WATANABE Add func1
|/
* 9c7ae76  2014-11-03 Takayuki WATANABE 2nd&3rd&4&5th commit
* 7714ceb  2014-11-03 Takayuki WATANABE 1st commit
```

### 問題4-6 
--------------------
#### シナリオ
function6の実装が困難だったため、途中経過を残すために`function.txt`に`f`,`u`,`n`,`c`,`6`をそれぞれ追記するごとにコミットログを残すことにした。`6`を追記した所で無事function6の実装が完了したがコミットログが汚くなってしまった。そこで、fucntion6の実装で残したコミットログを1つにまとてmasterブランチに統合した。function5の開発は次のコミットで完了したので、一時的コミットと1つにまとめてmasterブランチにマージした。

#### 問題
04_handle_branchのgitレポジトリの状態をAfter6からAfter7、After7からAfter8に変更せよ。

### <a name="ans4-6">問題4−6 解答</a>

After6からAfter7の状態にする。

```
➤  vim function.txt
➤  git add function.txt
➤
➤  git commit
[function6 2f33693] Add f
➤  vim function.txt
➤  git add function.txt
➤  git commit
[function6 96158ca] Add u
 1 file changed, 1 insertion(+), 1 deletion(-)
➤  vim function.txt
➤  git add function.txt
➤  git commit
[function6 636ef6a] Add n
 1 file changed, 1 insertion(+), 1 deletion(-)
➤  vim function.txt
➤  git add function.tt
fatal: pathspec 'function.tt' did not match any files
➤  git add function.txt
➤  git commit
[function6 f6252fb] Add c
 1 file changed, 1 insertion(+), 1 deletion(-)
➤  vim function.txt
➤  git add function.txt
➤  git commit
[function6 f3564de] Add 6
 1 file changed, 1 insertion(+), 1 deletion(-)
```
 次にAfter7からAfter8の状態にする　

```
➤  git rebase -i HEAD~5
===== rebase内容 =====
pick 2f33693 Add f
s 96158ca Add u
s 636ef6a Add n
s f6252fb Add c
s f3564de Add 6
=====================
➤  git checkout master
Switched to branch 'master'
[INSERT]:takayuki@IAC1-WATANABE
➤  git merge --no-ff function6
Merge made by the 'recursive' strategy.
 function.txt | 2 ++
 1 file changed, 2 insertions(+)
➤  git checkout function5
➤  vim function.txt
➤  git add function.txt
➤  git commit
[function5 bbd7409] Add func5
➤  git log --oneline
bbd7409 Add func5
a3fa652 Add temporary func5 commit
b94036a Add func4
7438105 Merge branch 'function2'
13713c4 Merge branch 'function3'
a119fb6 Add function3
ca3afef Add func2
04b7397 Merge branch 'function1'
a26701d Add func1
9c7ae76 2nd&3rd&4&5th commit
7714ceb 1st commit
➤  git rebase -i HEAD~2
===== rebase内容 =====
pick a3fa652 Add temporary func5 commit
s bbd7409 Add func5
=====================
➤  git log --oneline
ed0fd01 Add func5
b94036a Add func4
7438105 Merge branch 'function2'
13713c4 Merge branch 'function3'
a119fb6 Add function3
ca3afef Add func2
04b7397 Merge branch 'function1'
a26701d Add func1
9c7ae76 2nd&3rd&4&5th commit
7714ceb 1st commit
➤  git checkout master
➤  git merge --no-ff function5
＃ コンフリクトの解消
➤  vim function.txt
➤  git add function.txt
➤  git commit
➤  git graph
*   0feeeff  (HEAD, master) 2014-11-08 Takayuki WATANABE Merge branch 'function5'
|\
| * ed0fd01  (function5) 2014-11-08 Takayuki WATANABE Add func5
* |   d41d2ac  2014-11-08 Takayuki WATANABE Merge branch 'function6'
|\ \
| * | 44c4299  (function6) 2014-11-08 Takayuki WATANABE Add func6
| * | 63b305d  2014-11-08 Takayuki WATANABE Add func4
|/ /
| * b94036a  2014-11-08 Takayuki WATANABE Add func4
|/
*   7438105  2014-11-08 Takayuki WATANABE Merge branch 'function2'
|\
* \   13713c4  2014-11-08 Takayuki WATANABE Merge branch 'function3'
|\ \
| * | a119fb6  (function3) 2014-11-08 Takayuki WATANABE Add function3
|/ /
| * ca3afef  (function2) 2014-11-08 Takayuki WATANABE Add func2
|/
*   04b7397  2014-11-08 Takayuki WATANABE Merge branch 'function1'
|\
| * a26701d  (function1) 2014-11-08 Takayuki WATANABE Add func1
|/
* 9c7ae76  2014-11-03 Takayuki WATANABE 2nd&3rd&4&5th commit
* 7714ceb  2014-11-03 Takayuki WATANABE 1st commit
```
### Link
--------------------
 * Previous: [03_clean_up_commit : 過去のコミットをまとめて修正する、綺麗にまとめる](03_clean_up_commit.md)
  * Next: 
 * 目次: [README](README.md)
 