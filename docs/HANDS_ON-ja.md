# ハンズオンチュートリアル: Copilot Orchestra で TODO アプリフロントエンドを構築

このハンズオンガイドでは、GitHub Copilot Orchestra を使用して TODO アプリケーションのフロントエンドを構築する手順を説明します。React と TypeScript で複数の AI エージェントを活用して開発ワークフローを効率化する方法を学びます。

## 🎯 学習目標

このチュートリアルを完了すると、以下ができるようになります:
- 必要なツールを備えた dev container 環境をセットアップ
- Copilot Orchestra ワークフローの理解
- TypeScript でモダンな React UI を構築
- REST API バックエンドとの統合
- TODO 管理機能の実装
- レスポンシブデザインとユーザーインタラクションの追加
- テストを記述してコード品質を確保
- AI 支援ワークフローを使用してプルリクエストを作成

## 📚 前提条件

開始する前に、以下を確認してください:
- React と TypeScript の基本知識
- REST API の基本的な理解
- Docker Desktop がインストール済み
- Dev Containers 拡張機能を含む VS Code
- GitHub Copilot サブスクリプション
- GitHub アカウント

## 🏁 パート 1: 環境セットアップ

### ステップ 1.1: Dev Container でプロジェクトを開く

1. VS Code でこのプロジェクトを開く
2. "Reopen in Container" のプロンプトが表示されたら、**Reopen in Container** をクリック
   - または `F1` を押して `Dev Containers: Reopen in Container` を選択
3. コンテナのビルドを待つ(初回は約 2〜3 分)
4. 完了すると、以下を含む完全に構成された環境が利用可能になります:
   - Node.js 20
   - Git CLI
   - GitHub CLI (gh)
   - GitHub Copilot
   - Web Search for Copilot 拡張機能
   - ESLint & Prettier

### ステップ 1.2: 環境を確認

統合ターミナルを開いて実行:

```bash
# Node.js バージョンを確認
node --version  # v20.x.x が表示されるはず

# Git を確認
git --version

# GitHub CLI を確認
gh --version

# npm パッケージがインストールされているか確認
npm list
```

### ステップ 1.3: GitHub CLI を認証

```bash
gh auth login
```

プロンプトに従って GitHub アカウントで認証します。

## 🤖 パート 2: Copilot Orchestra を理解する

Copilot Orchestra は複数の専門化された AI エージェントを調整します:

```
ユーザーリクエスト
    ↓
[Orchestrator Agent] ← 現在ここ
    ↓
    ├─→ [Issue Agent] ────→ GitHub Issue を作成
    ↓
    ├─→ [Plan Agent] ─────→ 実装計画を設計
    ↓
    ├─→ [Impl Agent] ─────→ コードを記述
    ↓
    ├─→ [Review Agent] ───→ コードをレビューして改善
    ↓
    └─→ [PR Agent] ───────→ プルリクエストを作成
```

### 主要概念

1. **Orchestrator Agent**: 全体のワークフローを管理
2. **Issue Agent**: 要件を理解し、詳細なイシューを作成
3. **Plan Agent**: タスクを実行可能なステップに分解
4. **Implementation Agent**: 計画に従ってコードを記述
5. **Review Agent**: コード品質とベストプラクティスを確保
6. **PR Agent**: 包括的なプルリクエストを作成

## 🔨 パート 3: TODO フロントエンドを構築

### ステップ 3.1: 既存のコード構造を探索

まず、現在の構成を理解しましょう:

```bash
# 現在の構造を表示
tree -L 2 src/
```

以下が表示されます:
- `App.tsx` - メインアプリケーションコンポーネント
- `main.tsx` - アプリケーションのエントリーポイント
- `assets/` - 画像と静的リソース
- `components/` - React コンポーネント(これから作成)

### ステップ 3.2: 開発サーバーを起動

```bash
npm run dev
```

開発サーバーは `http://localhost:5173` で起動します。ブラウザが自動的に開くか、手動で URL にアクセスします。

### ステップ 3.3: Vite のセットアップを理解する

このプロジェクトでは以下のために Vite を使用しています:
- ⚡️ 超高速な HMR(ホットモジュールリプレースメント)
- 📦 最適化されたプロダクションビルド
- 🎨 組み込みの TypeScript サポート
- 🔧 シンプルな設定

### ステップ 3.4: プロジェクト構造の概要

```
gh-copilot-multirepo-demo-frontend/
├── src/
│   ├── App.tsx              # メインアプリコンポーネント
│   ├── main.tsx             # エントリーポイント
│   ├── components/          # React コンポーネント
│   │   ├── TodoList.tsx     # Todo リストコンポーネント
│   │   ├── TodoItem.tsx     # 個別の todo アイテム
│   │   └── TodoForm.tsx     # Todo 追加フォーム
│   ├── hooks/               # カスタム React フック
│   ├── services/            # API 通信
│   ├── types/               # TypeScript 型定義
│   └── utils/               # ヘルパー関数
├── public/                  # 静的アセット
└── index.html              # HTML テンプレート
```

## 🎨 パート 4: UI コンポーネントの構築

### ステップ 4.1: コンポーネントアーキテクチャの理解

TODO アプリは以下の主要コンポーネントで構成されます:

1. **App.tsx** - ルートコンポーネント、状態とレイアウトを管理
2. **TodoList.tsx** - todo のリストを表示
3. **TodoItem.tsx** - アクション付きの個別 todo アイテム
4. **TodoForm.tsx** - 新しい todo を作成するフォーム

### ステップ 4.2: バックエンドへの接続

フロントエンドはバックエンド API と通信します:

| フロントエンドアクション | バックエンドエンドポイント | メソッド |
|----------------------|---------------------|--------|
| Todo を読み込み | `/api/todos` | GET |
| Todo を作成 | `/api/todos` | POST |
| Todo を更新 | `/api/todos/:id` | PUT |
| Todo を削除 | `/api/todos/:id` | DELETE |

### ステップ 4.3: 状態管理戦略

このプロジェクトでは React の組み込み状態管理を使用します:
- `useState` - ローカルコンポーネント状態用
- `useEffect` - 副作用(API 呼び出し)用
- カスタムフック - 再利用可能なロジック用

## 🧪 パート 5: Copilot を使った作業

### ステップ 5.1: Copilot Chat を使用

1. Copilot Chat を開く (Mac では `Ctrl+Cmd+I`、Windows/Linux では `Ctrl+Shift+I`)
2. 試しに質問:
   ```
   @workspace todo アイテムを表示する新しい React コンポーネントを作成するにはどうすればよいですか?
   ```

3. Copilot がワークスペースを分析し、コンテキストに応じた提案を提供します

### ステップ 5.2: インライン提案

1. `src/App.tsx` を開く
2. コメントを入力開始: `// API から todo を取得する関数を作成`
3. `Tab` を押して Copilot の提案を受け入れるか、`Delegate to agent` を使用

### ステップ 5.3: タスクをオーケストレーターエージェントに依頼

1. Copilot Chat を開き、`orchestrator` を選択

2. `ステータスでフィルタリングできるグリッドレイアウトで todo を表示するコンポーネントを実装したい` と入力

3. 以下のような Todos が作成されることを確認

- issue エージェントで GitHub issue を作成
- plan エージェントで実装計画を立てる
- impl エージェントで実装を行う
- review エージェントでコードレビューを行う

**表記は異なることがある点にご留意ください。orchestrator エージェントが、各エージェントにタスクを分配していることが確認できればOKです。**

## 🎯 練習問題

### 演習 1: Todo の優先度を追加
色分けを伴う優先度システム(高、中、低)を実装します。

### 演習 2: 検索機能を実装
タイトルまたは説明で todo をフィルタリングする検索バーを追加します。

### 演習 3: ダークモードを追加
localStorage でテーマの永続化を行うダークモード切り替えを実装します。

### 演習 4: ダッシュボードを作成
統計情報(合計 todo、完了、保留中など)を表示するダッシュボードを構築します。

### 演習 5: アニメーションを追加
CSS トランジションまたは Framer Motion のようなライブラリを使用してスムーズなアニメーションを追加します。

## 🎨 スタイリングのベストプラクティス

### CSS Modules の使用
```tsx
import styles from './TodoItem.module.css'

function TodoItem() {
  return <div className={styles.todoItem}>...</div>
}
```

### レスポンシブデザイン
```css
/* モバイルファーストアプローチ */
.container {
  padding: 1rem;
}

@media (min-width: 768px) {
  .container {
    padding: 2rem;
  }
}
```

## 🧪 コンポーネントのテスト

### ステップ 6.1: コンポーネントテストの記述

```bash
# テスト依存関係をインストール
npm install --save-dev @testing-library/react @testing-library/jest-dom vitest
```

### ステップ 6.2: テストの例

```tsx
import { render, screen } from '@testing-library/react'
import TodoItem from './TodoItem'

test('todo アイテムをレンダリング', () => {
  render(<TodoItem title="テスト Todo" completed={false} />)
  expect(screen.getByText('テスト Todo')).toBeInTheDocument()
})
```

## 🐛 トラブルシューティング

### コンテナがビルドされない
- Docker が実行中か確認
- 再ビルドを試す: `Dev Containers: Rebuild Container`

### Copilot が動作しない
- サブスクリプションがアクティブか確認
- 拡張機能が有効か確認
- VS Code の再読み込みを試す

### API 接続の問題
- バックエンドサーバーが実行中か確認
- 設定内の API URL を確認
- バックエンドの CORS 設定を確認

### Vite ビルドエラー
- node_modules をクリアして再インストール: `rm -rf node_modules && npm install`
- TypeScript エラーを確認: `npm run build`

## 📚 ご参考

- [React ドキュメント](https://ja.react.dev/)
- [TypeScript ハンドブック](https://www.typescriptlang.org/ja/docs/)
- [Vite ガイド](https://ja.vite.dev/guide/)
- [GitHub Copilot ドキュメント](https://docs.github.com/ja/copilot)
- [copilot-orchestra](https://github.com/ShepAlderson/copilot-orchestra)

## 🚀 次のステップ

このチュートリアルを完了した後、以下を検討してください:
1. ユーザー認証の追加
2. WebSocket でのリアルタイム更新の実装
3. Service Worker でのオフラインサポートの追加
4. Azure Static Web Apps などのプラットフォームへのデプロイ
5. バックエンドの Web Push 通知機能との統合