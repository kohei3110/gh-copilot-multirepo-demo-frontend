# TODO アプリ フロントエンド - Copilot Orchestra デモ

**GitHub Copilot Orchestra** を使用して TODO アプリケーションのフロントエンドを構築する実践例です。このプロジェクトは、複数の専門化されたエージェントが協力して動作する AI 駆動な開発ワークフローを実証します。

## 概要

このリポジトリには、以下の技術を使用した TODO アプリケーションのフロントエンド実装が含まれています:
- **React** と **TypeScript**
- **Vite** による高速な開発と最適化されたビルド
- **GitHub Copilot Orchestra** による AI 支援開発

このプロジェクトは、Copilot Orchestra を使用して複数の AI エージェント(issue、plan、implementation、review、PR エージェント)を統制し、開発プロセスを効率化する方法を示します。

## はじめに

### 前提条件

- [Docker](https://www.docker.com/get-started) がインストール済み
- [Visual Studio Code](https://code.visualstudio.com/) と [Dev Containers 拡張機能](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
- GitHub アカウント

### Dev Container を使ったクイックスタート

1. **リポジトリをクローン**
   ```bash
   git clone https://github.com/kohei3110/gh-copilot-multirepo-demo-frontend.git
   cd gh-copilot-multirepo-demo-frontend
   ```

2. **Dev Container で開く**
   - VS Code を開く
   - `F1` または `Cmd+Shift+P` (Mac) / `Ctrl+Shift+P` (Windows/Linux) を押す
   - 選択: `Dev Containers: Reopen in Container`

3. **開発サーバーを起動**
   ```bash
   npm run dev
   ```

Dev Container には以下が含まれています:
- Git CLI
- GitHub CLI (gh)
- GitHub Copilot
- Web Search for Copilot 拡張機能
- ESLint & Prettier

## 機能

- TODO 管理のためのモダンな React UI
- 型安全性のための TypeScript
- 高速な HMR と最適化されたビルドのための Vite
- レスポンシブデザイン
- コード品質のための ESLint 設定
- 事前設定済みの dev container 環境

## プロジェクト構造

```
├── .devcontainer/          # Dev container 設定
├── public/                 # 静的アセット
├── src/
│   ├── App.tsx            # メインアプリケーションコンポーネント
│   ├── main.tsx           # アプリケーションのエントリーポイント
│   ├── assets/            # 画像やその他のアセット
│   └── components/        # React コンポーネント
├── index.html             # HTML テンプレート
└── vite.config.ts         # Vite 設定
```

## Copilot Orchestra について

Copilot Orchestra は、複数の AI エージェントを調整する高度なワークフローです:

1. **Issue エージェント**: 要件を分析し、詳細なイシューを作成
2. **Plan エージェント**: 実装計画を作成
3. **Implementation エージェント**: 計画に基づいてコードを記述
4. **Review エージェント**: コード品質をレビューして改善
5. **PR エージェント**: 包括的な説明を含むプルリクエストを作成

このアプローチにより、最小限の手動介入で高品質かつよく文書化されたコードが保証されます。

## 関連プロジェクト

- [バックエンドリポジトリ](https://github.com/kohei3110/gh-copilot-multirepo-demo-backend) - この TODO アプリケーションのバックエンド API

---

**注**: これは教育目的のデモンストレーションプロジェクトであり、GitHub Copilot Orchestra を使用した AI 支援開発のベストプラクティスを紹介しています。
