# 花粉エクスキュースジェネレーター — フロントエンド

花粉症・限界突破エクスキュース（言い訳）ジェネレーターのフロントエンドです。  
プロダクト全体の概要・セットアップ手順は [親リポジトリの README](https://github.com/hduehgw0/excuse-by-pollen) を参照してください。

## 技術スタック

| カテゴリ | 技術 |
|---|---|
| フレームワーク | Next.js 16 (App Router) |
| 言語 | TypeScript |
| スタイリング | Tailwind CSS v4 |
| アイコン | Font Awesome |
| OGP 画像生成 | @vercel/og |
| テスト | Playwright (E2E) |
| デプロイ | Vercel |

## ディレクトリ構成

```
front/
├── app/
│   ├── page.tsx          # メインページ（言い訳生成フォーム + 結果表示）
│   ├── layout.tsx        # アプリ共通レイアウト
│   ├── share/            # シェアページ（OGP 対応）
│   ├── api/
│   │   ├── ogp-image/    # OGP 画像生成ルート（@vercel/og）
│   │   ├── generate/     # モック API（バックエンド未起動時のフォールバック）
│   │   ├── retry/        # モック API（もっと盛る）
│   │   └── get-excuse/   # モック API（シェア用言い訳取得）
│   └── utils/            # ユーティリティ関数群
├── components/
│   └── ui/               # UI コンポーネント（Header, Footer, 左右パネル）
├── hooks/
│   ├── useGenerate.tsx   # 言い訳生成ロジック（API 呼び出し・状態管理）
│   ├── useGetExcuse.tsx  # シェア用言い訳取得ロジック
│   └── ui/               # UI カスタムフック（ドロップダウン, 外部クリック検知など）
├── tests/
│   └── generate.spec.ts  # Playwright E2E テスト
└── types/
    └── api.ts            # API レスポンス型定義
```

## 実装機能

- **言い訳生成**: 症状・相手・状況・花粉症レベル（1〜5）・ニュアンスを入力し、AI が言い訳を生成
- **もっと盛る**: 生成済みの言い訳に追加指示を与えて再生成
- **説得力スコア**: 生成した言い訳の説得力を AI が数値化して表示
- **読み上げ機能**: ブラウザの SpeechSynthesis API を使って言い訳を音声再生
- **位置情報 × 花粉バッジ**: 現在地の花粉指数・種類をバッジ表示
- **X シェア / OGP 画像生成**: @vercel/og を使ってサーバーサイドで OGP 画像を生成して X に投稿

## API 接続

`hooks/utils/APIUtil.tsx` で環境ごとに接続先を切り替えています。

| 環境 | 接続先 |
|---|---|
| 本番 | Render.com にデプロイされたバックエンド |
| 開発 (`npm run dev`) | `http://localhost:8000` |
| バックエンド未起動時のフォールバック | Next.js の `/api` モックルート |

## セットアップ

```bash
npm install
# または
bun install
```

## 開発サーバーの起動

```bash
npm run dev
# または
bun dev
```

[http://localhost:3000](http://localhost:3000) でアクセスできます。

## テスト

[Playwright](https://playwright.dev/) を使った E2E テストを実装しています。  
言い訳の生成・「もっと盛る」機能の動作を自動検証します。

```bash
# 初回のみ
npx playwright install

# テスト実行
npx playwright test tests/generate.spec.ts
```

## デプロイ

`main` ブランチへのマージ・プッシュで Vercel へ自動デプロイされます。
