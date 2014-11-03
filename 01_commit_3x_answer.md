01_commit_3xの解答
========

### 問題
--------------------
今、01_commit_3xのgitレポジトリはBeforeの状態である。  
コミットを3回行うことで、gitレポジトリをAfterの状態にせよ。


![代替テキスト](images/01_commit_3x.png)

### 解答
--------------------
```
vi commit.txt (2nd commitを追記)
git add commit.txt
git commit -m "2nd commit"
vi commit.txt (3rd commitを追記)
git add commit.txt
git commit -m "3rd commit"
vi commit.txt (4th commitを追記)
git add commit.txt
git commit -m "4th commit"
```