---
title: "HUGOを使ってブログを作る"
date: "2021-01-19 00:00:00+0900"
tags: ["HUGO"]
showToc: true
TocOpen: false
draft: false
---

## はじめに
以下の理由からHUGOを使用してブログを作ってみました！
- 個人的なアウトプットする場所が欲しい
- スタイル(CSS)を書くのは面倒くさい
- SSG(静的サイトジェネレータ)を試してみたい

[HUGO](https://gohugo.io)は静的サイトジェネレータです。  
Markdownで書かれたファイル（記事）をテンプレートエンジンを通してHTMLにしてくれます。
また、[テーマ](https://themes.gohugo.io)が多く公開されていて簡単にスタイルの整ったページを作れます。

## 導入
### インストール
```sh
brew install hugo
```



### 確認
```shell
hugo version
```  

```shell
❯ hugo version  
Hugo Static Site Generator v0.80.0/extended darwin/amd64 BuildDate: unknown
```

### テーマの適応
今回は[PaperMod](https://github.com/adityatelange/hugo-PaperMod)をテーマとして使用します。  
テーマのインストールについては[こちら](https://adityatelange.github.io/hugo-PaperMod/posts/papermod/papermod-installation) を参照してください。

### 記事ファイルの作成
```shell
hugo new <作成するファイルのパス名>
```
contentディレクトリ配下に`.md`ファイルが作成されます。  
テーマで使用されるparamsについては[こちら](https://adityatelange.github.io/hugo-PaperMod/posts/papermod/papermod-features/) を参照して適宜`.md`ファイルを変更してください。

## 公開する
今回はGitHub Pagesでホスティングすることにしました。  
また、記事を書いてPush時に自動でデプロイされるようにGitHub Actionsを使いました。
（自分は一つのリポジトリに静的コンテンツを生成するコードと生成された静的コンテンツを一緒に管理したかったので若干参考のリンクと異なります）

### [GitHub Pages](https://zenn.dev/nikaera/articles/hugo-github-actions-for-github-pages) でホスト
> [Hugo + GitHub Pages + GitHub Actions で独自ドメインのウェブサイトを構築する](https://docs.github.com/ja/github/working-with-github-pages/about-github-pages)

### GitHub Actionsで自動デプロイ
> [GitHub Actions による GitHub Pages への自動デプロイ](https://qiita.com/peaceiris/items/d401f2e5724fdcb0759d)

## さいごに
非常に簡単にビジュアルの良いブログが簡単に作れてしまいました！  
今回はGitHub Pagesにホストすることにしましたが他にもNetlifyやVercelでもホスティングすることができます。  
今後はこちらのブログに日々のアウトプットをしていこうと思います。  
このブログの[リポジトリ](https://github.com/taakuuyaa/taakuuyaa.github.io)