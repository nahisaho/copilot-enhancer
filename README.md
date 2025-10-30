# Copilot Enhancer

ソフトウェア開発を加速する18個の専門AIエージェント

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub Copilot](https://img.shields.io/badge/GitHub%20Copilot-Compatible-blue)](https://github.com/features/copilot)
[![Official Spec](https://img.shields.io/badge/Copilot-Official%20Spec-green)](https://docs.github.com/en/copilot/reference/custom-agents-configuration)

## 🚀 概要

Copilot Enhancerは、**GitHub Copilot公式仕様に準拠**した18個の専門AIエージェントを提供します。各エージェントはソフトウェア開発の特定領域に特化し、高品質な成果物を自動生成します。

### ✨ 特徴

- **🎯 専門性**: 各エージェントが特定領域に特化（アーキテクチャ、DB設計、テスト、セキュリティなど）
- **📊 高品質**: 実務で即使える成果物を生成（ドキュメント、コード、設定ファイル）
- **💬 対話型**: 1問1答形式で段階的に情報を収集し、最適な成果物を生成
- **🔧 実用的**: 実際のプロジェクトで使用可能なベストプラクティス
- **🌐 日本語対応**: 全エージェントが日本語でのやり取りに対応
- **✅ 公式準拠**: GitHub Copilot公式仕様に完全準拠

## 📋 エージェント一覧（18個）

### 🎭 統括エージェント
| # | エージェント | 専門領域 |
|---|-----------|---------|
| 1 | **Orchestrator AI** | 全エージェント統括・ワークフロー調整 |

**Orchestrator AI**は、17個の専門エージェントを自動的に選択・実行し、複雑なワークフローを管理します。プロジェクト全体の自動化に最適です。

### 🏗️ 設計・アーキテクチャ
| # | エージェント | 専門領域 |
|---|-----------|---------|
| 2 | **Requirements Analyst AI** | 要件分析・ユーザーストーリー作成 |
| 3 | **System Architect AI** | システムアーキテクチャ設計・ADR |
| 4 | **Database Schema Designer AI** | データベーススキーマ設計・ER図 |
| 5 | **API Designer AI** | REST/GraphQL API設計 |
| 6 | **UI/UX Designer AI** | UI/UX設計・ワイヤーフレーム |
| 7 | **Cloud Architect AI** | クラウドアーキテクチャ設計 |

### 💻 開発・実装
| # | エージェント | 専門領域 |
|---|-----------|---------|
| 8 | **Code Reviewer AI** | コードレビュー・品質チェック |
| 9 | **Bug Hunter AI** | バグ分析・デバッグ支援 |
| 10 | **Performance Optimizer AI** | パフォーマンス分析・最適化 |

### 🧪 品質保証
| # | エージェント | 専門領域 |
|---|-----------|---------|
| 11 | **Test Engineer AI** | テスト設計・自動生成 |
| 12 | **Quality Assurance AI** | 品質保証・テスト戦略 |
| 13 | **Security Auditor AI** | セキュリティ監査・脆弱性診断 |

### 🚀 運用・DevOps
| # | エージェント | 専門領域 |
|---|-----------|---------|
| 14 | **DevOps Engineer AI** | CI/CD・インフラ自動化 |
| 15 | **Observability Engineer AI** | 監視・ログ・メトリクス設計 |

### 📝 マネジメント・ドキュメント
| # | エージェント | 専門領域 |
|---|-----------|---------|
| 16 | **Project Manager AI** | プロジェクト管理・計画 |
| 17 | **Agile Coach AI** | スクラム・アジャイル支援 |
| 18 | **Technical Writer AI** | 技術文書作成・ドキュメント |

## 🎬 クイックスタート

### 前提条件

- GitHub Copilot のサブスクリプション
- 対応エディタ（VS Code、JetBrains IDEs等）

### インストール

```bash
# 1. リポジトリをクローンまたはダウンロード
git clone https://github.com/nahisaho/copilot-enhancer.git

# 2. プロジェクトのルートディレクトリに移動
cd copilot-enhancer

# 3. .github/agents/ ディレクトリが存在することを確認
ls .github/agents/

# 4. エディタを再起動してエージェントを有効化
```

### 基本的な使い方

#### 単一エージェントの利用

GitHub Copilot のチャット画面で `@workspace /` に続けてエージェント名を入力:

```
@workspace /system-architect
新規Webアプリケーションのアーキテクチャを設計してください
```

```
@workspace /database-schema-designer
ブログシステムのデータベーススキーマを設計してください
```

```
@workspace /code-reviewer
このファイルをレビューしてください
```

#### Orchestratorによる自動化（推奨）

複雑なプロジェクトや複数エージェントが必要な場合は、Orchestratorに任せることで自動的に最適なワークフローが実行されます：

```
@workspace /orchestrator
ECサイトを開発したい。要件定義からデプロイまで全て対応してください
```

```
@workspace /orchestrator
既存システムの品質を改善したい。セキュリティ、パフォーマンス、コード品質を診断してください
```

```
@workspace /orchestrator
APIを設計してドキュメントも作成してください
```

## 💬 対話型エージェント

全てのエージェントは**対話型実行**を採用しています：

### 対話フロー（5フェーズ）

1. **初回ヒアリング**: 基本情報を質問（プロジェクト概要、技術スタックなど）
2. **詳細ヒアリング**: 専門的な要件を深掘り（セキュリティ、パフォーマンスなど）
3. **確認フェーズ**: 収集した情報を要約して最終確認
4. **成果物生成**: ヒアリング内容に基づいて成果物を作成
5. **フィードバック**: 生成した成果物へのフィードバックを受け付け

### 対話例

```
👤 ユーザー: @workspace /database-schema-designer
データベースを設計してください

🤖 エージェント:
[フェーズ1] 質問 1/6: プロジェクト概要

まず、データベース設計の対象となるシステムについて教えてください。

どのようなシステムのデータベースを設計しますか？
a) ECサイト
b) SNS/コミュニティサイト
c) 業務管理システム
d) その他（具体的に教えてください）
```

## 🎯 使用例

### シナリオ1: 新規プロジェクトの立ち上げ（Orchestrator使用）

```
@workspace /orchestrator
新規ECサイトを開発します。
以下の機能が必要です：
- ユーザー登録・ログイン
- 商品検索・カート機能
- 決済機能（Stripe連携）
- 管理画面

要件定義からデプロイメント設定まで、全てのフェーズを実施してください。
```

**Orchestratorが自動実行する内容**:
1. Requirements Analyst: 機能要件・非機能要件定義
2. 並列実行: Database Schema Designer（DB設計） + API Designer（API設計） + UI/UX Designer（UI設計）
3. System Architect: 全体アーキテクチャ統合
4. DevOps Engineer: CI/CD構築
5. Test Engineer: テストコード生成
6. Security Auditor: セキュリティ監査
7. Technical Writer: ドキュメント作成

### シナリオ2: 手動で各エージェント実行（カスタマイズしたい場合）

```
# ステップ1: 要件分析
@workspace /requirements-analyst
プロジェクトの要件を整理してユーザーストーリーを作成してください

# ステップ2: アーキテクチャ設計
@workspace /system-architect
C4モデルでアーキテクチャを設計してください

# ステップ3: データベース設計
@workspace /database-schema-designer
ER図とDDLを生成してください

# ステップ4: API設計
@workspace /api-designer
RESTful APIとOpenAPI仕様書を作成してください

# ステップ5: CI/CD構築
@workspace /devops-engineer
GitHub ActionsでCI/CDパイプラインを構築してください
```

### シナリオ3: コード品質向上

```
# セキュリティチェック
@workspace /security-auditor
OWASP Top 10に基づいて脆弱性をチェックしてください

# コードレビュー
@workspace /code-reviewer
品質とパフォーマンスを重点的にレビューしてください

# テスト生成
@workspace /test-engineer
ユニットテストを生成してカバレッジを80%以上にしてください
```

### シナリオ4: ドキュメント整備

```
# README作成
@workspace /technical-writer
プロジェクトのREADMEを作成してください

# API仕様書生成
@workspace /api-designer
OpenAPI仕様書を生成してください

# セットアップガイド
@workspace /technical-writer
開発環境のセットアップガイドを作成してください
```

## 🏗️ プロジェクト構造

```
copilot-enhancer/
├── .github/
│   └── agents/               # GitHub Copilot公式仕様準拠のエージェント定義（18個）
│       ├── orchestrator.md
│       ├── requirements-analyst.md
│       ├── system-architect.md
│       ├── database-schema-designer.md
│       ├── api-designer.md
│       ├── uiux-designer.md
│       ├── cloud-architect.md
│       ├── code-reviewer.md
│       ├── bug-hunter.md
│       ├── performance-optimizer.md
│       ├── test-engineer.md
│       ├── quality-assurance.md
│       ├── security-auditor.md
│       ├── devops-engineer.md
│       ├── observability-engineer.md
│       ├── project-manager.md
│       ├── agile-coach.md
│       └── technical-writer.md
├── docs/                     # ドキュメント
│   ├── USER_GUIDE.md
│   ├── AGENT_REGISTRY.md
│   └── agent-roadmap.md
└── README.md
```

### ファイル形式

各エージェントファイルは**GitHub Copilot公式仕様**に準拠：

```yaml
---
name: "Agent Name"
description: "Agent description"
---

# Agent Prompt Content
（プロンプトの詳細な内容）
```

## 🛠️ カスタマイズ

### エージェントのカスタマイズ

`.github/agents/*.md` を直接編集してエージェントをカスタマイズできます。

```markdown
---
name: "Custom Agent"
description: "カスタムエージェントの説明"
tools: ["shell", "read", "edit", "search"]  # オプション（省略するとすべてのツールが有効）
---

# Custom Agent Prompt

あなたは「カスタムエージェント」です。
（プロンプトの内容）
```

### 新しいエージェントの追加

1. `.github/agents/` に新しいMarkdownファイル（`.md`）を作成
2. YAMLフロントマター（name, description）とプロンプトを記述
3. エディタを再起動

詳細は [GitHub Copilot公式ドキュメント](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/create-custom-agents) を参照してください。

## 📊 各エージェントの主な機能

### System Architect AI
- C4モデル（Context/Container/Component/Code）
- ADR（Architecture Decision Record）
- ATAM（トレードオフ分析）
- セキュリティアーキテクチャ
- 可観測性戦略

### Database Schema Designer AI
- ER図作成（Mermaid）
- 正規化分析（1NF〜BCNF）
- DDL生成
- インデックス戦略
- マイグレーション計画

### Code Reviewer AI
- OWASP Top 10チェック
- パフォーマンス分析
- 複雑度測定（Cyclomatic Complexity）
- リファクタリング提案
- ベストプラクティス検証

### Test Engineer AI
- ユニットテスト生成
- 統合テスト設計
- E2Eテストシナリオ
- モック・スタブ生成
- カバレッジ分析

### Technical Writer AI
- README作成
- API仕様書（OpenAPI）
- 開発者ガイド
- ユーザーマニュアル
- Changelog生成

### DevOps Engineer AI
- CI/CDパイプライン（GitHub Actions、GitLab CI）
- Docker/Kubernetes
- Terraform/CloudFormation
- 監視・アラート設定
- デプロイ戦略（Blue-Green、Canary）

### API Designer AI
- RESTful API設計
- GraphQLスキーマ
- OpenAPI 3.0仕様
- 認証・認可戦略（OAuth2、JWT）
- バージョニング戦略

### Security Auditor AI
- 脆弱性診断
- OWASP Top 10準拠
- 依存関係チェック
- セキュアコーディング
- コンプライアンス監査

### Requirements Analyst AI
- ユーザーストーリー作成
- ユースケース設計
- 受け入れ基準定義
- MoSCoW優先順位付け
- 要件トレーサビリティ

### Cloud Architect AI
- AWS/Azure/GCP設計
- Well-Architected Framework
- コスト最適化
- DR/BCP計画
- マルチクラウド戦略

### Project Manager AI
- WBS作成
- リスク管理
- 工数見積もり（PERT、三点見積もり）
- スプリント計画
- ステークホルダー管理

### Bug Hunter AI
- エラーログ分析
- スタックトレース解析
- 根本原因分析（5 Whys、RCA）
- 修正パッチ提案
- 再発防止策

### Performance Optimizer AI
- ボトルネック検出
- アルゴリズム計算量分析（Big-O）
- メモリ使用量最適化
- クエリ最適化（N+1問題解決）
- キャッシング戦略

### UI/UX Designer AI
- ワイヤーフレーム作成
- ユーザーフロー設計
- デザインシステム提案
- アクセシビリティチェック（WCAG 2.1）
- レスポンシブデザイン

### Observability Engineer AI
- ログ・メトリクス・トレース設計（OpenTelemetry）
- SLI/SLO定義
- Grafanaダッシュボード設計
- アラートルール作成
- インシデント対応プロセス

### Agile Coach AI
- スプリントプランニング
- レトロスペクティブ分析
- ベロシティ分析
- バーンダウンチャート生成
- ユーザーストーリー分割

### Quality Assurance AI
- テスト戦略立案（テストピラミッド）
- 品質メトリクス定義
- 品質ゲート設計
- 欠陥密度分析
- 静的解析ツール設定

### Orchestrator AI
- エージェント自動選択
- ワークフロー調整
- タスク分解
- 結果統合
- 進捗管理

## 📖 ドキュメント

- **[利用者ガイド](docs/USER_GUIDE.md)** - 詳細な使い方とサンプル
- **[エージェントレジストリ](docs/AGENT_REGISTRY.md)** - 全エージェントの詳細仕様
- **[開発ロードマップ](docs/agent-roadmap.md)** - 今後の拡張計画
- **[GitHub Copilot公式ドキュメント](https://docs.github.com/en/copilot/reference/custom-agents-configuration)** - カスタムエージェント仕様

## 🔍 技術仕様

- **GitHub Copilot公式仕様準拠**: [Creating Custom Agents](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/create-custom-agents)
- **必須フィールド**: `name`, `description`
- **オプションフィールド**: `tools` (省略時はすべてのツールが有効)
- **ファイル形式**: Markdown（`.md`）with YAMLフロントマター
- **配置場所**: `.github/agents/`

## 🤝 コントリビューション

プルリクエストを歓迎します。大きな変更の場合は、まずIssueで議論してください。

### コントリビューションガイドライン

1. GitHub Copilot公式仕様に準拠すること
2. 対話型フロー（1問1答形式）を含めること
3. 成果物をファイル出力すること
4. 日本語で記述すること

## 📄 ライセンス

[MIT License](LICENSE)

## 🙏 謝辞

このプロジェクトは GitHub Copilot の機能を活用しています。

## 📮 お問い合わせ

- **Issues**: [GitHub Issues](https://github.com/nahisaho/copilot-enhancer/issues)
- **Discussions**: [GitHub Discussions](https://github.com/nahisaho/copilot-enhancer/discussions)

---

**Made with ❤️ and GitHub Copilot**
