---
title: "macOS：SonomaでGolangのデバッグツールdelveがうまく動作しなかった件について"
emoji: "💭"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Go", "Golang", "delve"]
published: true
---

## 概要

MacOS を Sonoma にアップデートしたら Golang のデバッグツール delve がうまく動作しなくなったのですが、
解決できたので、記事に残しておきます！
同じ問題にぶつかった方の少しでもお役に立てればと...🙏

### エラー内容

```
Unable to retrieve goroutines: write tcp 127.0.0.1:56960->127.0.0.1:56961: write: broken pipe
Detaching and terminating target process
```

## 結論

delve の issue に同様の問題を抱えた方の投稿が上がってました！
https://github.com/go-delve/delve/issues/3538

どうやら CommandLineTools のバージョンが影響しているようでしたので、再インストールする必要がありそうです。

### 手順 1

xcode を再インストールする。
私は xcode をアンインストールしてたので、この方法を使いましたが、手順 2 の方が簡単です。

### 手順 2

CommandLineTools のパスをチェック

```sh
xcode-select -p
```

上記で出てきたディレクトリを手動で削除

```sh
sudo rm -rf {上記で見つかったパス}
# sudo rm -rf /Library/Developer/CommandLineTools ←こんな感じ
```

再インストール

```sh
xcode-select --install
```

## 終わり

かなり悩んでいたので、解決できてよかったです。
MacOS のアップデートは慎重に。勉強になりました！
