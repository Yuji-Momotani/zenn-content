---
title: "React、Next.jsのレンダリングパターンまとめ"
emoji: "🕌"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["React", "Next.js"]
published: true
---

## 概要

Next.js を学ぶ際に必須となってくるレンダリングのパターンの知識をまとめておこうと思います。

## レンダリングの種類

- CSR：クライアントサイドレンダリング
- SSR：サーバーサイドレンダリング
- SG（SSG）：静的サイト生成
- ISR：イングリメンタル静的再生成

### CSR

全ての処理をブラウザ（クライアント）側で行う。
React 単体での実装がこれに当たる。
SPA とだいたい同意義になるが、違いとしては、SPA はアプリケーションの種類、CSR はレンダリングの種類として語られる。

#### CSR のメリット

- サーバー側の負荷が少ない
- 初期表示以降は高速に動作する

#### CSR のデメリット

- 大規模のシステムになるほど、初回描画には時間がかかる。（全ての js ファイル等を初回でダウンロードしてくる必要があるため）
- HTML が読み込まれた時点では、ほとんど空のファイルとなっているため、クローラーによっては SEO の問題が発生する可能性がある。（JavaScript で HTML を生成するため）

#### CSR のイメージ

![CSRイメージ](/images/1c7fd2facdca65/csr_image.png)

- Web サーバーより各ファイルを落としてきた後、クライアント側で JavaScript によって、HTML をレンダリングする。
- データフェッチや更新の場合、API サーバーとのやり取りはクライアントが直接行う。

#### CSR に最適なアプリケーション例

動的でインタラクティブなアプリケーション。

- Google Map
- 進捗管理、ホワイトボード系のアプリ（Miro とか）

### SSR

HTML の生成と全ての処理をサーバー（Node.js）側で実行する。
Next.js は基本的には SSR もしくは SG のレンダリングとなる。

#### SSR のメリット

- サーバー側で生成済みの HTML が返ってくるため、SEO 対策になる。
- 初回読み込みは CSR より早い。（クライアントの要求に応じてサーバー側で HTML が生成されるため、初回に全ての JS ファイルをダウンロードする必要がない）
- クライアントのスペックに依存しない。

#### SSR のデメリット

- ページ遷移毎にリクエストを投げるため、サーバーに負荷はかかる。
- 初回読み込み以降は CSR より速度が遅くなる。

#### SSR のイメージ

![SSR](/images/1c7fd2facdca65/ssr_image.png)

- リクエスト毎に Web サーバーでレンダリングし、それをクライアントに返す。
- データフェッチや更新の場合、API サーバーとのやり取りは Web サーバー側（Node.js）で行う。

#### SSR に最適なアプリケーション例

動的で SEO 対策が必要なアプリケーション

- ブログ
- EC サイト

### SG

ビルド時に HTML ファイルが構築される。
クライアントからリクストが来ると生成済みの HTML ファイルを返す。

#### SG のメリット

- 初回表示は CSR、SSR と比較すると最も早い（ビルド済みのファイルを返すだけのため）
- 2 回目以降は CSR と同様高速に動作する。
- SEO 対策になる

#### SG のデメリット

- 動的コンテンツとの相性が悪い（ビルド時に全ての HTML を構築しているために動的には処理できない）

#### SG のイメージ

![SGイメージ](/images/1c7fd2facdca65/sg_image.png)

- ビルド時にレンダリングし、それをクライアントに返す。よって、データフェッチはビルド時のみ

#### SG に最適なアプリケーション例

- 静的サイトなら SG 一択
- LP、ドキュメントサイトなど

### ISR

ビルド時に HTML を構築。（ここまでは SG と同様）
一定時間後にアクセスがあった場合には、生成済みの HTML ファイルを返却しつつ、サーバー側で HTML を再構築することによって、次回アクセスがあった場合には最新の HTML ファイルが返ってくる。
（SSR と SG のいいとこどり）
（revalidate というプロパティで上記の一定時間を設定する）

#### ISR のメリット

- SG と同じくビルド時に HTML が構築されるため、高速に動作する。
- 厳格に動的処理はされないが、一定時間後には最新の状態になる。
- 要は SSR と SG のいいとこどり

#### ISR のデメリット

- 即時性や情報の正確性が求められる場合には向かない。
- サーバー側の設定が手間だが、Vercel を使えば問題ない。

#### ISR のイメージ

![ISRイメージ1](/images/1c7fd2facdca65/isr_image1.png)

- ビルド時にレンダリングし、それをクライアントに返す。ビルド時のみデータフェッチ
- 一定時間経過後にリクエストがあった場合は、再ビルド。

以下、各時間軸でみる ISR

![ISRイメージ2](/images/1c7fd2facdca65/isr_image2.png)

- 20 秒時点でデータの更新がかかる。
- 40 秒時点でリクエストが来ても更新されていないページを返す。
- 70 秒時点でリクエストが来た場合、revalidate で指定された 60 秒を超えているので、更新されていないページを返しつつ、ページが再ビルドされる。
- 70 秒以降にリクエストが来たら、新しいページにして返す。

#### ISR に最適なアプリケーション例

- SSR より更新頻度は高くはないが、更新される場合があるアプリケーション
- ブログ、記事投稿系のアプリケーション

## まとめ

基本的には React で

- CSR

Next.js で

- SSR
- SG
  のレンダリングが用いられる。
  Next.js の場合、指定すれば CSR としてレンダリングさせることもできる。
  また、ページ毎にどのレンダリングとするか決めることも可能。

ISR は SSR と SG のいいとこどりだけど、Vercel にデプロイする場合のみ使用した方が良い。
