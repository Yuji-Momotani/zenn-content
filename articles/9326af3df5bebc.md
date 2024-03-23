---
title: "【メモ】SlackのRSSアプリとIFTTTを使用して、特定の文字列にヒットするフィードのみを取得する"
emoji: "💬"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["slack", "rss", "IFTTT"]
published: true
---

<!-- ## アプリの追加

![alt text](/images/9326af3df5bebc/image1.png)

![alt text](/images/9326af3df5bebc/image2.png)

- 追加ボタン押下後、ブラウザに遷移

![alt text](/images/9326af3df5bebc/image3.png)

- Slack に追加

![alt text](/images/9326af3df5bebc/image4.png)

- フィード URL 追加
- 登録するチャンネルを選択

→「このフィードを購読する」

## Slack 上から RSS アプリの設定、確認など

- フィード追加

```
/feed subscribe <フィード url>
```

- フィードのリスト

```
/feed list
```

- フィードの削除

```
/feed remove <フィードID>
``` -->

# 概要

IFTTT と RSS を使用して特定の文字でフィルタリングした記事のみを Slack に通知する。

# Slack の設定

記事の通知専用チャンネルを作成したい場合はあらかじめ作っておく。

# IFTTT の設定

## サインイン

[IFTTT](https://ifttt.com/)のトップ画面に遷移。
アカウントがなければ「Get Started」からサインインする。

![alt text](/images/9326af3df5bebc/image5.png)

- 好きな方法でサインインする

## アプレットの作成

- 「Create」から作成
  ![alt text](/images/9326af3df5bebc/image6.png)

- 「If This」を add
  ![alt text](/images/9326af3df5bebc/image7.png)

- 「RSS Feed」を選択
  ![alt text](/images/9326af3df5bebc/image8.png)

- 「New feed item matches」をクリック
  ![alt text](/images/9326af3df5bebc/image9.png)

- KeyWord と Feed URL の入力
  ![alt text](/images/9326af3df5bebc/image10.png)

- 「Then That」を add
  ![alt text](/images/9326af3df5bebc/image11.png)

- Slack を選択
  ![alt text](/images/9326af3df5bebc/image12.png)

- 「Post to Chanel」を選択 → Connect
  ![alt text](/images/9326af3df5bebc/image13.png)

- 送信したいチャンネルを指定 → Create action → Continue
  ![alt text](/images/9326af3df5bebc/image14.png)

- Applet のタイトルを入力し、Finish
  ![alt text](/images/9326af3df5bebc/image15.png)

## 余談

IFTTT は無料版だと 2 つまでアプレットとやらを作成できるらしい。有料版にすると Pro で 20 個まで、Pro+で無制限となる。
