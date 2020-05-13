# nuxtjs-spa-example

> Nuxt.js の実装例

## 目次

- [Build Setup](#Build-setup)
- [ルートディレクトリのファイル・フォルダ説明](#ルートディレクトリのファイル・フォルダ説明)
- [関連リンク](#関連リンク)
- [その他](#その他)

## Build Setup

```bash
# 依存パッケージの追加
$ yarn install

# localhost:3000で開発用Webサーバを起動（ホットリロードに対応）
$ yarn dev

# productionビルドとその実行
$ yarn build
$ yarn start

# 静的projectの作成
$ yarn generate
```

## ルートディレクトリのファイル・フォルダ説明

- assets

  - css/sass など、webpack でコンパイルしたいファイルを置くディレクトリ
  - js ファイルは/plugin に置くことが多いので、主にスタイル関連や web フォントなどを置くことになる
  - ファイルパスは「~/assets/」から指定する

- components

  - 共通部品となる vue ファイルを置くディレクトリ
  - 単体でページを作るのではなく、layouts や pages 配下の vue ファイルから import されて部品として呼び出される

- layouts

  - アプリケーション全体の共通フレーム（メニューとメイン部分の骨組みなど）を定義したファイルを置くディレクトリ

- middleware

  - ページが描画される前に実行したい共通処理（ログインチェックなど）を書いた js を置くディレクトリ

- pages

  - メインとなる画面を置くディレクトリ
  - デフォルトではここの vue ファイル名が URL として使われる（アドレスバーに表示される）

- plugins

  - js の共通部品などを置くディレクトリ

- static

  - 画像などの静的ファイルを置くディレクトリ
  - webpack でコンパイルしないファイルを置く
  - assets と異なり、アクセスするときのパスは「~/static/」ではなく static をルートとして「/」から始める

- store

  - 複数ページを跨ぐデータを扱う変数を定義する
  - ここで定義した変数の値はページ遷移しても初期化されないので初期化忘れに注意
  - spa なので通常はリロード(F5)すると消えてしまう（回避するには [vuex-persistedstate](#関連リンク) などを使う）

- nuxt.config.js

  - nuxt の設定ファイル
  - middleware や plugin の読み込み、ビルド時の設定など諸々ここで指定する

- .env

  - [@nuxtjs/dotenv](#関連リンク)が使用する設定ファイル
  - 環境変数の代わりにこのファイルで認証情報などを定義する

    書き方

    ```Text
    key=value
    ```

（注）node.js が標準で使用するファイル（packaga.json など）はここでは扱いません

## 関連リンク

- [vuex-persistedstate](https://github.com/robinvdvleuten/vuex-persistedstate)
- [@nuxtjs/dotenv](https://github.com/nuxt-community/dotenv-module)

## その他

For detailed explanation on how things work, check out [Nuxt.js docs](https://nuxtjs.org).
