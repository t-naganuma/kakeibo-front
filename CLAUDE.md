# CLAUDE.md

このファイルは、Claude Code (claude.ai/code) がこのリポジトリで作業する際のガイダンスを提供します。

## 言語設定

**重要: このコードベースでのやり取りは常に日本語で行ってください。** これは日本の家計簿アプリケーションであり、特に指示がない限り、すべてのコミュニケーションは日本語で行う必要があります。

## プロジェクト概要

モダンな TanStack ライブラリとツールを使用して構築された、React ベースの家計簿アプリケーションのフロントエンドです。

## 開発コマンド

### アプリケーションの起動
```bash
npm run dev          # ポート3000で開発サーバーを起動
npm run build        # 本番用ビルド（vite build && tsc を実行）
npm run preview      # 本番ビルドのプレビュー
```

### テスト
```bash
npm run test         # Vitest ですべてのテストを実行
```

### コード品質
```bash
npm run format       # Biome でコードをフォーマット
npm run lint         # Biome でコードをリント
npm run check        # リントとフォーマットチェックの両方を実行
```

**重要**: Biome はインデントに**タブ**、JavaScript/TypeScript には**ダブルクォート**を使用します。lefthook を介して、pre-commit フックが自動的にステージングされたファイルに対して `biome check --write` を実行します。

## 技術スタック

- **React 19** with TypeScript
- **TanStack Router** (v1) - コードベースルーティング（現在の設定、ファイルベースへの移行も可能）
- **Vite 7** - ビルドツールと開発サーバー
- **Tailwind CSS 4** - `@tailwindcss/vite` プラグイン経由でのスタイリング
- **Vitest** - `@testing-library/react` を使用したテストフレームワーク
- **Biome** - リントとフォーマット
- **Lefthook** - Git フック管理

## アーキテクチャとパターン

### ルーティング設定

アプリは TanStack Router の**コードベースルーティング**を使用しています：
- ルートは `src/main.tsx` で定義されています
- ルートルートには、ネストルーティング用の `<Outlet />` と `<TanStackRouterDevtools />` が含まれています
- 新しいルートは `createRoute()` で作成し、`rootRoute.addChildren([])` 経由で `routeTree` に追加します

ルート構造の例：
```typescript
const newRoute = createRoute({
  getParentRoute: () => rootRoute,
  path: '/path',
  component: ComponentName,
})
// その後、rootRoute.addChildren([indexRoute, newRoute]) に追加
```

ファイルベースルーティングへの移行については、README.md の包括的なガイドを参照してください。

### パスエイリアス

プロジェクトは `src/` ディレクトリへのエイリアスとして `@/` を使用しています：
- `vite.config.ts`（resolve.alias）と `tsconfig.json`（paths）の両方で設定されています
- 例: `import Header from '@/components/Header'`

### コンポーネント構造

- `src/components/` - 再利用可能な UI コンポーネント（例: モバイル対応サイドバー付きの Header）
- コンポーネントは Tailwind CSS でスタイリングされています
- アイコンは `lucide-react` を使用

### 状態管理

現在は React の組み込み state（useState）を使用しています。README には以下の統合例が含まれています：
- TanStack Query（サーバー状態用）
- TanStack Store（グローバルクライアント状態用）

### テスト

- テストファイルは `.test.tsx` 拡張子を使用
- テストは Vitest + Testing Library を使用
- 例: `src/App.test.tsx`

## 開発ノート

### TypeScript 設定

- strict モードが有効で、追加チェック（`noUnusedLocals`、`noUnusedParameters`）が設定されています
- `moduleResolution: "bundler"` と `verbatimModuleSyntax` を使用
- ターゲット: ES2022

### Biome 設定

- `biome.json` のパターンに一致するファイルのみをチェック（`src/**`、`index.html`、`vite.config.js` を含む）
- 除外: `src/routeTree.gen.ts`、`src/styles.css`
- インポートの整理が自動的に有効化されています

### ビルドノート

- ビルドコマンドは、型チェックのため `vite build` と `tsc` の両方を実行します
- 本番ビルドは `dist/` に出力されます
- 開発サーバーはポート 3000 で実行されます（package.json にハードコードされています）

## 現在の状態

アプリは以下の状態でスキャフォールディングされています：
- TanStack/React ブランディングを含む基本的なホームページ（`App.tsx`）
- スライドアウトナビゲーション付きのレスポンシブヘッダーコンポーネント
- Web Vitals レポートのセットアップ
- テストインフラストラクチャの構築完了

プロジェクトは家計簿機能の実装準備が整っています。
