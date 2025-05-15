# cfp-cli1

Vite で作った React のプロジェクトを
**GitHub 連携せずに CLI だけで**
Cloudflare Pages にアップロードしてみるテスト。

## 手順

```sh
# プロジェクトつくって動かす
pnpm create vite cfp-cli1 --template react-ts
cd cfp-cli1
pnpm up -L
pnpm dev

# wrangler追加
pnpm add -D wrangler
## リフォーマットしたり, run-scripts修正したり
## wrangler.toml 書く
# `git init` して mainブランチにしておく

# Cloudflare Pages にプロジェクト作る
wrangler pages project create cfp-cli1 --production-branch main # → run-scriptsに書いた。`pnpm run create`
## 初めての時はここで authentication 処理があるはず

# ビルドしてデプロイ
pnpm build
wrangler pages deploy   # wrangler.toml 見てやってくれる → run-scriptsに書いた。`pnpm run deploy`

# んでプレビュー用のブランチで作業
git switch -c dev
## いろいろ修正...
pnpm build
pnpm run deploy		# カレントブランチの名前でプレビューデプロイされる

# いろいろ動かしたら
# Cloudflare Pages から消す
wrangler pages project delete cfp-cli1
```

## 注意

deploy 前の build 忘れがち。
いっぺんにやる run-script 書いとく

## この方法の良い点・悪い点

- **良い点**
  - GitHub 連携だと、README 変更しただけでもビルド処理動くのが心苦しい。それがなくなる
  - GitHub で Cloudflare 連携の設定しなくていいから楽
  - Cloudflare 側の設定がはぶけて楽
- **悪い点**
  - wrangler まわりのパッケージが若干大きい
  - 修正後 build & deploy を忘れる(超ありそう)
  - (嘘でした。`wrangler pages deploy` で勝手にやってくれる。) functions が別ビルドらしい。参考: [functions build](https://developers.cloudflare.com/workers/wrangler/commands/#functions-build)
    - functions build はサイズ見るぐらい? 2023 年 10 月ごろに Free では 3MB、paid では 10GB limit

## 参考

[Commands - Wrangler · Cloudflare Workers docs](https://developers.cloudflare.com/workers/wrangler/commands/#pages)
