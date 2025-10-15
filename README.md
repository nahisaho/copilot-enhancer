# Copilot Enhancer

ソフトウェア開発を加速する17個の専門AIエージェント

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub Copilot](https://img.shields.io/badge/GitHub%20Copilot-Compatible-blue)](https://github.com/features/copilot)

## 🚀 概要

Copilot Enhancerは、GitHub Copilot内で動作する17個の専門AIエージェントを提供します。各エージェントはソフトウェア開発の特定領域に特化し、高品質な成果物を自動生成します。

### ✨ 特徴

- **🎯 専門性**: 各エージェントが特定領域に特化（アーキテクチャ、DB設計、テスト、セキュリティなど）
- **📊 高品質**: 実務で即使える成果物を生成（ドキュメント、コード、設定ファイル）
- **🔧 実用的**: 実際のプロジェクトで使用可能なテンプレートとベストプラクティス
- **🌐 日本語対応**: 全エージェントが日本語でのやり取りに対応
- **🔄 拡張可能**: 新しいエージェントの追加が容易

## 📋 エージェント一覧

### 🎭 統括エージェント
| # | エージェント | スラッグ | 専門領域 |
|---|-----------|---------|---------|
| 0 | **Orchestrator AI** | `@orchestrator` | 全エージェント統括・ワークフロー調整 |

**Orchestrator AI**は、16個の専門エージェントを自動的に選択・実行し、複雑なワークフローを管理します。プロジェクト全体の自動化に最適です。

### コア開発エージェント
| # | エージェント | スラッグ | 専門領域 |
|---|-----------|---------|---------|
| 1 | **System Architect AI** | `@architect` | システムアーキテクチャ設計 |
| 2 | **Database Schema Designer AI** | `@db-schema` | データベーススキーマ設計 |
| 3 | **Code Reviewer AI** | `@code-review` | コードレビュー・品質チェック |
| 4 | **Test Engineer AI** | `@test-engineer` | テスト設計・自動生成 |
| 5 | **Bug Hunter AI** | `@bug-hunter` | バグ分析・デバッグ支援 |
| 6 | **Performance Optimizer AI** | `@performance` | パフォーマンス分析・最適化 |

### ドキュメント・仕様エージェント
| # | エージェント | スラッグ | 専門領域 |
|---|-----------|---------|---------|
| 7 | **Technical Writer AI** | `@tech-writer` | 技術文書作成 |
| 8 | **Requirements Analyst AI** | `@requirements` | 要件分析・仕様策定 |
| 9 | **UI/UX Designer AI** | `@uiux` | UI/UX設計・ワイヤーフレーム |

### DevOps・インフラエージェント
| # | エージェント | スラッグ | 専門領域 |
|---|-----------|---------|---------|
| 10 | **DevOps Engineer AI** | `@devops` | CI/CD・インフラ自動化 |
| 11 | **Cloud Architect AI** | `@cloud` | クラウドアーキテクチャ設計 |
| 12 | **Observability Engineer AI** | `@observability` | 監視・ログ・メトリクス設計 |

### API・セキュリティエージェント
| # | エージェント | スラッグ | 専門領域 |
|---|-----------|---------|---------|
| 13 | **API Designer AI** | `@api-design` | API設計・仕様書生成 |
| 14 | **Security Auditor AI** | `@security` | セキュリティ監査・脆弱性診断 |

### プロジェクト管理・品質エージェント
| # | エージェント | スラッグ | 専門領域 |
|---|-----------|---------|---------|
| 15 | **Project Manager AI** | `@pm` | プロジェクト管理・計画 |
| 16 | **Agile Coach AI** | `@agile` | スクラム・アジャイル支援 |
| 17 | **Quality Assurance AI** | `@qa` | 品質保証・テスト戦略 |

## 🎬 クイックスタート

### 前提条件

- GitHub Copilot のサブスクリプション
- 対応エディタ（VS Code、JetBrains IDEs等）

### インストール

```bash
# リポジトリをクローン
git clone https://github.com/your-username/copilot-enhancer.git
cd copilot-enhancer

# エディタを再起動してエージェントを有効化
```

### 基本的な使い方

#### 単一エージェントの利用

GitHub Copilot のチャット画面で `@` に続けてエージェント名を入力:

```
@architect 新規Webアプリケーションのアーキテクチャを設計してください
```

```
@db-schema ブログシステムのデータベーススキーマを設計してください
```

```
@code-review このファイルをレビューしてください
```

#### Orchestratorによる自動化（推奨）

複雑なプロジェクトや複数エージェントが必要な場合は、Orchestratorに任せることで自動的に最適なワークフローが実行されます：

```
@orchestrator ECサイトを開発したい。要件定義からデプロイまで全て対応してください
```

```
@orchestrator 既存システムの品質を改善したい。セキュリティ、パフォーマンス、コード品質を診断してください
```

```
@orchestrator APIを設計してドキュメントも作成してください
```

## 📖 ドキュメント

- **[利用者ガイド](docs/USER_GUIDE.md)** - 詳細な使い方とサンプル
- **[エージェントレジストリ](docs/AGENT_REGISTRY.md)** - 全エージェントの詳細仕様と選択ガイド
- **[エージェント開発ロードマップ](docs/agent-roadmap.md)** - 全27エージェントの計画

## 🎯 使用例

### シナリオ1: 新規プロジェクトの立ち上げ（Orchestrator使用）

```
@orchestrator 新規ECサイトを開発します。
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
3. DevOps Engineer: CI/CD構築
4. Test Engineer: テストコード生成
5. Security Auditor: セキュリティ監査
6. Technical Writer: ドキュメント作成

### シナリオ1-B: 手動で各エージェント実行（カスタマイズしたい場合）

```
# ステップ1: 要件分析
@requirements プロジェクトの要件を整理してユーザーストーリーを作成してください

# ステップ2: アーキテクチャ設計
@architect C4モデルでアーキテクチャを設計してください

# ステップ3: データベース設計
@db-schema ER図とDDLを生成してください

# ステップ4: API設計
@api-design RESTful APIとOpenAPI仕様書を作成してください

# ステップ5: CI/CD構築
@devops GitHub ActionsでCI/CDパイプラインを構築してください
```

### シナリオ2: コード品質向上

```
# セキュリティチェック
@security OWASP Top 10に基づいて脆弱性をチェックしてください

# コードレビュー
@code-review 品質とパフォーマンスを重点的にレビューしてください

# テスト生成
@test-engineer ユニットテストを生成してカバレッジを80%以上にしてください
```

### シナリオ3: ドキュメント整備

```
# README作成
@tech-writer プロジェクトのREADMEを作成してください

# API仕様書生成
@api-design OpenAPI仕様書を生成してください

# セットアップガイド
@tech-writer 開発環境のセットアップガイドを作成してください
```

## 🏗️ プロジェクト構造

```
copilot-enhancer/
├── .copilot/
│   └── agents/              # エージェント定義（17個）
│       ├── system-architect-ai.yaml
│       ├── database-schema-designer.yaml
│       ├── code-reviewer.yaml
│       ├── test-engineer.yaml
│       ├── bug-hunter.yaml
│       ├── performance-optimizer.yaml
│       ├── technical-writer.yaml
│       ├── requirements-analyst.yaml
│       ├── uiux-designer.yaml
│       ├── devops-engineer.yaml
│       ├── cloud-architect.yaml
│       ├── observability-engineer.yaml
│       ├── api-designer.yaml
│       ├── security-auditor.yaml
│       ├── project-manager.yaml
│       ├── agile-coach.yaml
│       └── quality-assurance.yaml
├── prompts/                 # プロンプト定義（16個）
│   ├── *.md (各エージェント用)
├── templates/               # テンプレート集
│   ├── er-diagram-template.mmd
│   ├── table-definition-template.md
│   ├── migration-plan-template.md
│   └── index-strategy-template.md
├── docs/                    # ドキュメント
│   ├── USER_GUIDE.md
│   ├── agent-roadmap.md
│   └── ONEROSTER_API_HUB_DEVELOPMENT_GUIDE.md
└── README.md
```

## 🛠️ カスタマイズ

### エージェントのカスタマイズ

`.copilot/agents/*.yaml` と `prompts/*.md` を編集してエージェントをカスタマイズできます。

```yaml
# .copilot/agents/custom-agent.yaml
name: "Custom Agent"
slug: "custom"
description: "カスタムエージェントの説明"
entrypoint: "prompts/custom-agent.md"
# ...
```

### 新しいエージェントの追加

1. `.copilot/agents/` にYAML設定ファイルを作成
2. `prompts/` にプロンプトファイル（Markdown）を作成
3. エディタを再起動

詳細は [エージェント開発ロードマップ](docs/agent-roadmap.md) を参照してください。

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
- 複雑度測定
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
- デプロイ戦略

### API Designer AI
- RESTful API設計
- GraphQLスキーマ
- OpenAPI 3.0仕様
- 認証・認可戦略
- バージョニング

### Security Auditor AI
- 脆弱性診断
- OWASP Top 10
- 依存関係チェック
- セキュアコーディング
- コンプライアンス監査

### Requirements Analyst AI
- ユーザーストーリー
- ユースケース設計
- 受け入れ基準
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
- 工数見積もり
- スプリント計画
- ステークホルダー管理

### Bug Hunter AI
- エラーログ分析
- スタックトレース解析
- 根本原因分析（5 Whys）
- 修正パッチ提案
- 再発防止策

### Performance Optimizer AI
- ボトルネック検出
- アルゴリズム計算量分析（Big-O）
- メモリ使用量最適化
- クエリ最適化（N+1問題）
- キャッシング戦略

### UI/UX Designer AI
- ワイヤーフレーム作成
- ユーザーフロー設計
- デザインシステム提案
- アクセシビリティチェック（WCAG）
- レスポンシブデザイン

### Observability Engineer AI
- ログ・メトリクス・トレース設計
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

## 🤝 コントリビューション

プルリクエストを歓迎します。大きな変更の場合は、まずIssueで議論してください。

## 📄 ライセンス

[MIT License](LICENSE)

## 🙏 謝辞

このプロジェクトは GitHub Copilot の機能を活用しています。

## 📮 お問い合わせ

- **Issues**: [GitHub Issues](https://github.com/your-username/copilot-enhancer/issues)
- **Discussions**: [GitHub Discussions](https://github.com/your-username/copilot-enhancer/discussions)

---

**Made with ❤️ and GitHub Copilot**
