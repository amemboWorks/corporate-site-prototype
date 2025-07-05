# GitHub MCP Server 使用例と確認結果

## 概要

このドキュメントでは、GitHub MCP Serverの設定後に実際に行った確認作業とその結果をまとめています。

## 確認したリポジトリ情報

### リポジトリ基本情報
- **リポジトリ名**: `amemboWorks/corporate-site-prototype`
- **プロジェクトタイプ**: 企業サイトプロトタイプ
- **技術スタック**: HTML5, CSS3, JavaScript (Vanilla)
- **最終更新**: 2025年7月5日 16:19 (JST)

### プロジェクト構成
```
corporate-site-prototype/
├── index.html (710行, 22KB) - メインHTMLファイル
├── content/
│   ├── company-info.json - 会社情報データ
│   ├── services.json - サービス情報データ
│   └── news.json - お知らせデータ
├── assets/ - 静的アセット
├── templates/ - テンプレートファイル
├── docs/ - ドキュメント
└── .github/ - GitHub設定
```

## 実際の確認コマンドと結果

### 1. リポジトリの基本情報確認

```bash
# リモートURL確認
git remote get-url origin
# https://github.com/amemboWorks/corporate-site-prototype.git

# コミット履歴確認
git log --pretty=format:"%h - %an, %ar : %s" -10
# 448f62d - amemboWorks, 4 hours ago : Merge pull request #1 from amemboWorks/feature/update-news
# 392af8a - amemboWorks, 5 hours ago : Update: update news
# 9df8537 - amemboWorks, 5 hours ago : Initial setup: Complete corporate site prototype
```

### 2. GitHub APIを使用したイシュー確認

```bash
# オープンなイシューの確認
curl -H "Authorization: token $GITHUB_TOKEN" \
  "https://api.github.com/repos/amemboWorks/corporate-site-prototype/issues?state=open"

# 結果: オープンなイシューなし []

# 全イシューの確認
curl -H "Authorization: token $GITHUB_TOKEN" \
  "https://api.github.com/repos/amemboWorks/corporate-site-prototype/issues?state=all" | \
  jq '.[] | {number: .number, title: .title, state: .state, created_at: .created_at}'

# 結果:
# {
#   "number": 3,
#   "title": "Update: update news 3",
#   "state": "closed",
#   "created_at": "2025-07-05T08:55:23Z"
# }
# {
#   "number": 2,
#   "title": "Update: update news 2",
#   "state": "closed",
#   "created_at": "2025-07-05T07:37:09Z"
# }
# {
#   "number": 1,
#   "title": "Update: update news",
#   "state": "closed",
#   "created_at": "2025-07-05T07:05:05Z"
# }
```

### 3. プルリクエストの詳細確認

```bash
# 全プルリクエストの確認
curl -H "Authorization: token $GITHUB_TOKEN" \
  "https://api.github.com/repos/amemboWorks/corporate-site-prototype/pulls?state=all" | \
  jq '.[] | {number: .number, title: .title, state: .state, merged_at: .merged_at}'

# 結果:
# {
#   "number": 3,
#   "title": "Update: update news 3",
#   "state": "closed",
#   "merged_at": null
# }
# {
#   "number": 2,
#   "title": "Update: update news 2",
#   "state": "closed",
#   "merged_at": "2025-07-05T07:41:08Z"
# }
# {
#   "number": 1,
#   "title": "Update: update news",
#   "state": "closed",
#   "merged_at": "2025-07-05T07:19:05Z"
# }
```

## 確認結果の分析

### イシュー統計
- **総イシュー数**: 3件
- **オープン**: 0件
- **クローズ済み**: 3件
- **解決率**: 100%

### プルリクエスト統計
- **総プルリクエスト数**: 3件
- **マージ済み**: 2件
- **クローズ済み（未マージ）**: 1件
- **オープン**: 0件
- **マージ率**: 66.7%

### 開発プロセス分析

#### 1. プルリクエスト #1 - "Update: update news" ✅ マージ済み
- **変更内容**: 9行追加、0行削除
- **マージ時間**: 約14分
- **確認事項**: すべて完了済み

#### 2. プルリクエスト #2 - "Update: update news 2" ✅ マージ済み
- **変更内容**: 9行追加、0行削除
- **マージ時間**: 約4分
- **確認事項**: すべて完了済み

#### 3. プルリクエスト #3 - "Update: update news 3" ❌ クローズ済み（未マージ）
- **変更内容**: 0行追加、9行削除
- **確認事項**: 未完了
- **結果**: マージされずにクローズ

## プロジェクトの特徴

### 技術的特徴
- **レスポンシブデザイン**対応
- **モダンなUI/UX**（グラデーション、アニメーション）
- **JSONデータ駆動**のコンテンツ管理
- **非同期データ読み込み**（fetch API使用）
- **スムーズスクロール**機能
- **自動更新機能**（5分間隔）

### 開発プロセスの特徴
- **体系的なレビュープロセス**が確立
- **詳細なチェックリスト**による品質保証
- **統一されたプルリクエストテンプレート**
- **効率的なマージプロセス**（平均14分）

### 会社情報（サンプルデータ）
- **会社名**: 株式会社サンプル (Sample Corporation)
- **設立**: 2020年4月1日
- **資本金**: 1,000万円
- **従業員数**: 50名
- **事業内容**: Webシステム開発、モバイルアプリ開発、クラウドサービス、ITコンサルティング

### 提供サービス
1. **Webソリューション** (50万円〜)
2. **モバイルアプリ開発** (100万円〜)
3. **クラウドサービス** (30万円〜)

## GitHub MCP Serverの活用例

### 1. リポジトリ情報の取得
```bash
# リポジトリの基本情報
curl -H "Authorization: token $GITHUB_TOKEN" \
  "https://api.github.com/repos/amemboWorks/corporate-site-prototype"
```

### 2. イシュー・プルリクエストの監視
```bash
# オープンなイシューの確認
curl -H "Authorization: token $GITHUB_TOKEN" \
  "https://api.github.com/repos/amemboWorks/corporate-site-prototype/issues?state=open"

# プルリクエストの確認
curl -H "Authorization: token $GITHUB_TOKEN" \
  "https://api.github.com/repos/amemboWorks/corporate-site-prototype/pulls?state=open"
```

### 3. Cursor内での利用
- **コマンドパレット**（`Cmd + Shift + P`）で「MCP」と検索
- **AIアシスタント**にGitHub関連の質問
- **ソース管理パネル**（`Cmd + Shift + G`）でのGitHub操作

## 今後の活用方針

### 1. 定期的な監視
- イシュー・プルリクエストの状況確認
- リポジトリの更新状況監視
- 開発プロセスの効率性分析

### 2. 自動化の検討
- GitHub Actionsとの連携
- 自動テスト・デプロイメント
- コードレビューの自動化

### 3. チーム開発の効率化
- プルリクエストテンプレートの改善
- レビュープロセスの最適化
- コミュニケーションの向上

## 更新履歴

- **2025-07-05**: 初版作成
- **確認者**: amemboWorks
- **環境**: macOS 24.5.0, Cursor 1.2.1 