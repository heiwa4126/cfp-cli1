# cfp-cli1

Vite で作った React のプロジェクトを
GitHub 連携せずに
Cloudflare Pages にアップロードしてみるテスト

## 手順

```sh
# プロジェクトつくる
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
# ビルドしてデプロイ
pnpm build
wrangler pages deploy   # wrangler.toml 見てやってくれる → run-scriptsに書いた。`pnpm run deploy`

# んでプリビュー用のブランチで作業
git switch -c dev
## いろいろ作業
pnpm build
pnpm run deploy
```

TODD: 上記 `wrangler pages ...` のやつは run-scripts に書いておく。→ 書いた
