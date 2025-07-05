# GitHub MCP Server 設定手順

## 概要

GitHub MCP Serverは、CursorでGitHubの機能をより高度に利用するためのサーバーです。MCP（Model Context Protocol）を使用して、AIアシスタントがGitHubリポジトリの情報を取得したり、操作したりできるようになります。

## 前提条件

- macOS 24.5.0
- Node.js v24.3.0以上
- npm v11.4.2以上
- GitHubアカウント
- GitHub Personal Access Token

## 設定手順

### 1. 環境確認

```bash
# Node.jsバージョン確認
node --version
# v24.3.0

# npmバージョン確認
npm --version
# v11.4.2
```

### 2. GitHub MCP Serverのインストール

```bash
# グローバルインストール
npm install -g @modelcontextprotocol/server-github
```

インストール結果:
```
added 46 packages in 2s
9 packages are looking for funding
npm fund for details
```

### 3. GitHub Personal Access Tokenの作成

#### 3.1 GitHubでのトークン作成手順

1. **GitHubにログイン**
2. **Settings → Developer settings → Personal access tokens → Tokens (classic)**
3. **Generate new token → Generate new token (classic)**
4. **必要な権限を選択**：
   - `repo` (リポジトリへのフルアクセス)
   - `read:org` (組織の読み取り)
   - `read:user` (ユーザー情報の読み取り)
5. **トークンを生成してコピー**

#### 3.2 環境変数の設定

```bash
# .zshrcに環境変数を追加
echo 'export GITHUB_TOKEN="your_github_token_here"' >> ~/.zshrc

# 環境変数を再読み込み
source ~/.zshrc

# 環境変数の確認
echo $GITHUB_TOKEN
```

### 4. Cursorの設定

#### 4.1 Cursor設定ディレクトリの作成

```bash
mkdir -p ~/.cursor
```

#### 4.2 MCP設定ファイルの作成

`~/.cursor/mcp_servers.json` を作成:

```json
{
  "mcpServers": {
    "github": {
      "command": "mcp-server-github",
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

### 5. 動作確認

#### 5.1 インストール確認

```bash
# MCP Serverのヘルプ確認
mcp-server-github --help
```

#### 5.2 GitHub API接続確認

```bash
# リポジトリ情報の取得テスト
# GitHubリポジトリ「amemboWorks/corporate-site-prototype」の情報を教えて
curl -H "Authorization: token $GITHUB_TOKEN" \
  "https://api.github.com/repos/amemboWorks/corporate-site-prototype"
```

## 利用可能な機能

### 1. リポジトリ情報の取得

```bash
# オープンなイシューの確認
# このリポジトリのオープンなイシューを表示して
curl -H "Authorization: token $GITHUB_TOKEN" \
  "https://api.github.com/repos/amemboWorks/corporate-site-prototype/issues?state=open"

# プルリクエストの確認
# このリポジトリのプルリクエストを確認して
curl -H "Authorization: token $GITHUB_TOKEN" \
  "https://api.github.com/repos/amemboWorks/corporate-site-prototype/pulls?state=all"
```

### 2. Cursor内での利用

- **コマンドパレット**（`Cmd + Shift + P`）で「MCP」と検索
- **AIアシスタント**にGitHub関連の質問
- **ソース管理パネル**（`Cmd + Shift + G`）でのGitHub操作

## 設定完了後の確認事項

### ✅ 完了した作業
1. **Node.js環境**: v24.3.0（確認済み）
2. **GitHub MCP Server**: グローバルインストール完了
3. **GitHub Personal Access Token**: 環境変数に設定済み
4. **Cursor設定**: MCP設定ファイル作成済み

### 🔄 次のステップ
1. **Cursorを再起動**してMCP設定を読み込ませる
2. **コマンドパレット**（`Cmd + Shift + P`）で「MCP」と検索してGitHub関連の機能を確認
3. **AIアシスタント**にGitHubリポジトリの情報を問い合わせてみる

## トラブルシューティング

### よくある問題

#### 1. MCP Serverが見つからない
```bash
# パスの確認
which mcp-server-github

# 再インストール
npm uninstall -g @modelcontextprotocol/server-github
npm install -g @modelcontextprotocol/server-github
```

#### 2. GitHub API接続エラー
```bash
# トークンの確認
echo $GITHUB_TOKEN

# トークンの権限確認
curl -H "Authorization: token $GITHUB_TOKEN" \
  "https://api.github.com/user"
```

#### 3. CursorでMCP機能が表示されない
- Cursorを完全に再起動
- 設定ファイルの構文エラーを確認
- 環境変数が正しく設定されているか確認

## 参考リンク

- [GitHub MCP Server Documentation](https://github.com/modelcontextprotocol/server-github)
- [MCP Protocol Documentation](https://modelcontextprotocol.io/)
- [GitHub Personal Access Tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)

## 更新履歴

- **2025-07-05**: 初版作成
- **設定者**: amemboWorks
- **環境**: macOS 24.5.0, Cursor 1.2.1 