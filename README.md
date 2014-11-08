git-quiz
========

職場の人にgitの使い方を効率良く学習してもらうために作りました。  
git初心者〜中級者の方は練習に使ってください。

## 対象者 
* Gitに関する基本的な知識はあるが実践練習が足りない人
* 以下2つの内いずれかを通読している人
  * [サルでもわかるGit入門](http://www.backlog.jp/git-guide/) 
  * [Gitをはじめからていねいに](https://github.com/Shinpeim/introduction-to-git)

## Git configの設定
問題中のgitコマンドは以下の設定を採用。

```
$ git config --global user.name Takayuki WATANABE
$ git config --global user.email takanabe.w@gmail.com
$ git config --global color.ui auto
$ git config --global core.editor vim
$ git config --global alias.graph 'log --graph --date-order --all --pretty=format:'%h %Cred%d %Cgreen%ad %Cblue%cn %Creset%s' --date=short'
```

## 使い方
git-quizをダウンロードしたいディレクトリにて以下のコマンドを実行する。


```
$ git clone --recursive https://github.com/takanabe/git-quiz.git
```

ダウンロードしたレポジトリにはそれぞれの問題に対応するディレクトリを用意しました。ディレクトリの中には問題に必要なファイルが全て詰め込まれている（問題中のBeforeの状態になっています）ので、苦手な問題を繰り返し解いて快適なGit lifeを送くれるように頑張ってください！

## 問題一覧
### 初級 : 基本のコマンドを学ぶ
#### ローカルレポジトリの操作
 * [01_commit_3x : コミットを3回実施する](01_commit_3x.md)  
 * [02_amend_commit_log : 直近のコミット、コミットログを修正する](02_amend_commit_log.md)
 * [03_clean_up_commit : 過去のコミットをまとめて修正する、綺麗にまとめる](03_clean_up_commit.md)
 * [04_handle_branch : ブランチを使いこなす](04_handle_branch.md) 
 

## ライセンス
<a rel="license" href="http://creativecommons.org/licenses/by-nd/4.0/"><img alt="クリエイティブ・コモンズ・ライセンス" style="border-width:0" src="https://i.creativecommons.org/l/by-nd/4.0/88x31.png" /></a><br />この 作品 は <a rel="license" href="http://creativecommons.org/licenses/by-nd/4.0/">クリエイティブ・コモンズ 表示 - 改変禁止 4.0 国際 ライセンスの下に提供されています。</a>
