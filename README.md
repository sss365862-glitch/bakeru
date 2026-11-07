# 思考災害

小さな不安を入力すると卵が孵化し、「次の思考」を観測するたびに怪物が進化して、最終形態（第4形態）で爆発する1ページのWebデモ。

## ファイル構成

| ファイル | 中身 | 直すとき |
|---|---|---|
| index.html | 画面の部品の配置 | 部品を増やす・減らす |
| style.css | 色・大きさ・演出の速さ | 見た目を変える |
| config.js | 待ち時間、接頭語、感情5系統（記号・色・基本名・画像）、予備ルート、NGワード | 文章・名前・画像を変える |
| script.js | ボタンの動き、演出、Geminiへの問い合わせ | 動きを変える |
| api/generate.js | Vercel側でGeminiを呼ぶ処理と指示文 | AIの出す内容を変える |

**`generate.js` は必ず `api` フォルダの中に置く**（トップに置くと404になる）。

## 環境変数（Vercel）

| 名前 | 中身 |
|---|---|
| GEMINI_API_KEY | Google AI Studio で発行したAPIキー（必須） |
| GEMINI_MODEL | モデル名（任意。未設定なら `gemini-3.5-flash-lite`） |

## Vercelでの公開

1. このフォルダの中身をGitHubのリポジトリのトップにアップロード（`api` フォルダごと）
2. VercelでそのリポジトリをImport
3. 左のサイドバーの「Environment Variables」に `GEMINI_API_KEY` を登録
4. 「Deployments」から Redeploy（環境変数だけを変えたとき）
5. 確認：`公開URL/api/generate` を開いて `{"error":"POST only"}` が出ればOK

GitHubにファイルを上げ直すと、Vercelは自動で公開し直す。

## ローカルでの起動

`index.html` を開くだけでは `/api/generate` が動かないので、毎回「AIが応答しなかった」扱いになり予備ルートで完走する（図形の確認はこれでできる）。
AI込みで試すときは、Vercelに公開して確認する。

## 画像の差し替え

1. 画像を `img` フォルダに入れる（例：`img/aseri1.png`）
2. `config.js` の `EMOTIONS` の、その感情の `images` の該当形態に入れる
   ```js
   "焦り": { shape: "triangle", color: "#f5c518", base: "アセリム", images: ["img/aseri1.png", "img/aseri2.png", "img/aseri3.png", "img/aseri4.png"] },
   ```
3. 空欄や読み込み失敗の形態は図形で表示される。吸収・拡大・白い光・爆発の演出は画像でも同じ

画像は正方形・背景透過（PNG）を推奨。
