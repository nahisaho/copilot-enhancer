# Copilot Enhancer 利用者ガイド

## 📚 目次

1. [概要](#概要)
2. [セットアップ](#セットアップ)
3. [エージェント一覧](#エージェント一覧)
4. [使い方](#使い方)
5. [各エージェントの詳細](#各エージェントの詳細)
6. [実践例](#実践例)
7. [トラブルシューティング](#トラブルシューティング)
8. [FAQ](#faq)

---

## 概要

Copilot Enhancerは、GitHub Copilot内で動作する17個の専門AIエージェントを提供します。各エージェントはソフトウェア開発の特定領域に特化し、高品質な成果物を自動生成します。

### 🎯 主な特徴

- **専門性**: 各エージェントが特定領域に特化
- **実用性**: 実務で即使える高品質な成果物
- **一貫性**: 統一されたインターフェースと出力形式
- **拡張性**: 新しいエージェントの追加が容易

---

## セットアップ

### 前提条件

- GitHub Copilot のサブスクリプション
- 対応エディタ（VS Code、JetBrains IDEs、Neovim等）
- GitHub Copilot 拡張機能がインストール済み

### インストール手順

1. **プロジェクトのクローン**
   ```bash
   git clone https://github.com/your-username/copilot-enhancer.git
   cd copilot-enhancer
   ```

2. **エージェント設定の配置**

   すでに `.github/agents/` ディレクトリにエージェント定義が配置されています。

3. **GitHub Copilot の再起動**

   エディタを再起動するか、GitHub Copilot を再読み込みしてエージェントを有効化します。

4. **動作確認**

   GitHub Copilot のチャット画面で `@` を入力すると、利用可能なエージェント一覧が表示されます。

---

## エージェント一覧

| # | エージェント名 | 呼び出しコマンド | 専門領域 | 主な用途 |
|---|--------------|----------------|---------|---------|
| 1 | System Architect AI | `@workspace /system-architect` | アーキテクチャ設計 | C4モデル、ADR、技術選定 |
| 2 | Database Schema Designer AI | `@workspace /database-schema-designer` | DB設計 | ER図、DDL、マイグレーション |
| 3 | Code Reviewer AI | `@workspace /code-reviewer` | コードレビュー | 品質チェック、セキュリティスキャン |
| 4 | Test Engineer AI | `@workspace /test-engineer` | テスト設計 | ユニット/統合/E2Eテスト生成 |
| 5 | Bug Hunter AI | `@workspace /bug-hunter` | デバッグ | エラーログ分析、根本原因特定 |
| 6 | Performance Optimizer AI | `@workspace /performance-optimizer` | 最適化 | ボトルネック検出、計算量改善 |
| 7 | Technical Writer AI | `@workspace /technical-writer` | ドキュメント作成 | API仕様書、README、ガイド |
| 8 | Requirements Analyst AI | `@workspace /requirements-analyst` | 要件分析 | ユーザーストーリー、仕様策定 |
| 9 | UI/UX Designer AI | `@workspace /uiux-designer` | UI/UX設計 | ワイヤーフレーム、デザインシステム |
| 10 | DevOps Engineer AI | `@workspace /devops-engineer` | DevOps | CI/CD、Docker、K8s、IaC |
| 11 | Cloud Architect AI | `@workspace /cloud-architect` | クラウド設計 | AWS/Azure/GCP、コスト最適化 |
| 12 | Observability Engineer AI | `@workspace /observability-engineer` | 可観測性 | ログ・メトリクス・トレース、SLI/SLO |
| 13 | API Designer AI | `@workspace /api-designer` | API設計 | REST/GraphQL、OpenAPI仕様 |
| 14 | Security Auditor AI | `@workspace /security-auditor` | セキュリティ | 脆弱性診断、OWASP対応 |
| 15 | Project Manager AI | `@workspace /project-manager` | プロジェクト管理 | WBS、リスク管理、工数見積もり |
| 16 | Agile Coach AI | `@workspace /agile-coach` | アジャイル | スプリント計画、レトロスペクティブ |
| 17 | Quality Assurance AI | `@workspace /quality-assurance` | 品質保証 | テスト戦略、品質メトリクス |

---

## 使い方

### 基本的な使用方法

1. **エージェントの呼び出し**

   GitHub Copilot のチャット画面で `@workspace /` に続けてエージェント名を入力します。

   ```
   @workspace /code-reviewer
   このファイルをレビューしてください
   ```

2. **スラッシュコマンドの使用**

   各エージェントは専用のスラッシュコマンドを提供しています。

   ```
   @db-schema /er
   @test-engineer /unit calculate_discount
   @devops /docker python flask
   ```

3. **会話スターターの活用**

   各エージェントには推奨される質問パターン（スターター）が用意されています。

### エージェントの選び方

| やりたいこと | 推奨エージェント |
|------------|---------------|
| アーキテクチャ全体を設計したい | `@workspace /system-architect` |
| データベーススキーマを設計したい | `@workspace /database-schema-designer` |
| コードをレビューしてほしい | `@workspace /code-reviewer` |
| テストコードを生成したい | `@workspace /test-engineer` |
| バグを解析・修正したい | `@workspace /bug-hunter` |
| パフォーマンスを最適化したい | `@workspace /performance-optimizer` |
| APIドキュメントを作成したい | `@workspace /technical-writer` |
| 要件を整理したい | `@workspace /requirements-analyst` |
| UI/UXを設計したい | `@workspace /uiux-designer` |
| CI/CDパイプラインを構築したい | `@workspace /devops-engineer` |
| クラウドインフラを設計したい | `@workspace /cloud-architect` |
| 監視・ログ基盤を構築したい | `@workspace /observability-engineer` |
| REST APIを設計したい | `@workspace /api-designer` |
| セキュリティ診断をしたい | `@workspace /security-auditor` |
| プロジェクト計画を立てたい | `@workspace /project-manager` |
| スプリント計画を立てたい | `@workspace /agile-coach` |
| テスト戦略を策定したい | `@workspace /quality-assurance` |

---

## 各エージェントの詳細

### 1. System Architect AI (`@workspace /system-architect`)

**専門領域**: システムアーキテクチャ設計

**主な機能**:
- C4モデル（Context/Container/Component/Code）の作成
- ADR（Architecture Decision Record）の生成
- 技術選定とトレードオフ分析
- セキュリティアーキテクチャ設計
- 可観測性戦略

**スラッシュコマンド**:
- `/c4` - C4モデル生成
- `/adr` - ADR作成
- `/atam` - トレードオフ分析

**使用例**:
```
@workspace /system-architect
新規ECサイトのアーキテクチャを設計してください。
主要機能: ユーザー管理、商品管理、注文処理、決済連携
非機能要件: 可用性99.9%、ピーク時1000req/sec
```

---

### 2. Database Schema Designer AI (`@workspace /database-schema-designer`)

**専門領域**: データベーススキーマ設計

**主な機能**:
- ER図作成（Mermaid形式）
- 正規化分析（1NF〜BCNF）
- DDL生成（CREATE TABLE文）
- インデックス戦略設計
- マイグレーション計画

**スラッシュコマンド**:
- `/er` - ER図生成
- `/ddl` - DDL生成
- `/normalize` - 正規化分析
- `/migrate` - マイグレーションスクリプト生成
- `/index` - インデックス戦略提案

**使用例**:
```
@workspace /database-schema-designer
ブログシステムのデータベーススキーマを設計してください。
エンティティ: ユーザー、投稿、コメント、タグ
要件: 投稿の全文検索、タグによる絞り込み、1億件以上のスケーラビリティ
```

---

### 3. Code Reviewer AI (`@workspace /code-reviewer`)

**専門領域**: コードレビュー・品質チェック

**主な機能**:
- セキュリティ脆弱性検出（OWASP Top 10）
- パフォーマンス問題検出
- コード品質分析（複雑度、可読性）
- ベストプラクティス違反検出
- リファクタリング提案

**スラッシュコマンド**:
- `/review` - 包括的レビュー
- `/security` - セキュリティ重点レビュー
- `/performance` - パフォーマンス分析
- `/refactor` - リファクタリング提案
- `/metrics` - コードメトリクス分析

**使用例**:
```
@workspace /code-reviewer
このPythonファイルをレビューしてください。
特にセキュリティとパフォーマンスを重点的にチェックしてください。

[コードを貼り付け]
```

---

### 4. Test Engineer AI (`@workspace /test-engineer`)

**専門領域**: テスト設計・自動テスト生成

**主な機能**:
- ユニットテスト生成（境界値、エッジケース）
- 統合テスト設計
- E2Eテストシナリオ作成
- モック・スタブ生成
- テストカバレッジ分析

**スラッシュコマンド**:
- `/unit` - ユニットテスト生成
- `/integration` - 統合テスト生成
- `/e2e` - E2Eテスト生成
- `/mock` - モック生成
- `/coverage` - カバレッジ分析
- `/testdata` - テストデータ生成

**使用例**:
```
@workspace /test-engineer
この関数のユニットテストを生成してください（pytest使用）。
境界値とエッジケースを網羅してください。

def calculate_discount(purchase_amount, is_member):
    if purchase_amount < 0:
        raise ValueError("Invalid amount")
    if is_member:
        return purchase_amount * 0.1
    elif purchase_amount >= 5000:
        return purchase_amount * 0.05
    return 0
```

---

### 5. Technical Writer AI (`@workspace /technical-writer`)

**専門領域**: 技術文書作成

**主な機能**:
- README作成
- API仕様書生成（OpenAPI）
- 開発者ガイド作成
- コードドキュメント生成
- ユーザーマニュアル作成

**スラッシュコマンド**:
- `/readme` - README生成
- `/api-doc` - API仕様書生成
- `/setup-guide` - セットアップガイド生成
- `/changelog` - 変更履歴生成

**使用例**:
```
@workspace /technical-writer
このプロジェクトのREADMEを作成してください。
プロジェクト名: Task Manager API
技術スタック: Python 3.11, FastAPI, PostgreSQL, Docker
主な機能: タスクCRUD、ユーザー認証、タスクの共有
```

---

### 6. DevOps Engineer AI (`@workspace /devops-engineer`)

**専門領域**: CI/CD・インフラ自動化

**主な機能**:
- CI/CDパイプライン設計（GitHub Actions、GitLab CI）
- Dockerfile生成
- Kubernetes マニフェスト生成
- Terraform/CloudFormation コード生成
- 監視・アラート設定

**スラッシュコマンド**:
- `/cicd` - CI/CDパイプライン生成
- `/docker` - Dockerfile生成
- `/k8s` - Kubernetesマニフェスト生成
- `/iac` - IaCコード生成

**使用例**:
```
@workspace /devops-engineer
GitHub Actionsで以下のCI/CDパイプラインを構築してください。
1. プルリクエスト時: Lint、ユニットテスト、セキュリティスキャン
2. mainブランチマージ時: Docker イメージビルド、ECRプッシュ、ECS デプロイ
言語: Python 3.11
テストフレームワーク: pytest
```

---

### 7. API Designer AI (`@workspace /api-designer`)

**専門領域**: API設計

**主な機能**:
- RESTful API設計
- GraphQL スキーマ設計
- OpenAPI 3.0仕様生成
- API認証・認可戦略
- バージョニング戦略

**スラッシュコマンド**:
- `/rest` - REST API設計
- `/graphql` - GraphQLスキーマ設計
- `/openapi` - OpenAPI仕様生成

**使用例**:
```
@workspace /api-designer
以下のリソースでRESTful APIを設計してください。
リソース: ユーザー、タスク、プロジェクト
要件:
- JWT認証
- ページネーション対応
- フィルタリング・ソート機能
- レート制限（1000req/hour）
OpenAPI仕様書も生成してください。
```

---

### 8. Security Auditor AI (`@workspace /security-auditor`)

**専門領域**: セキュリティ監査・脆弱性診断

**主な機能**:
- OWASP Top 10チェック
- 脆弱性スキャン（SQLi、XSS、CSRF等）
- 認証・認可の評価
- 依存関係の脆弱性チェック
- セキュアコーディング提案

**スラッシュコマンド**:
- `/scan` - セキュリティスキャン
- `/owasp` - OWASP Top 10チェック
- `/auth-review` - 認証・認可レビュー
- `/dependencies` - 依存関係チェック

**使用例**:
```
@workspace /security-auditor
このログイン処理のセキュリティをチェックしてください。
OWASP Top 10に基づいて脆弱性を指摘してください。

[コードを貼り付け]
```

---

### 9. Requirements Analyst AI (`@workspace /requirements-analyst`)

**専門領域**: 要件分析・仕様策定

**主な機能**:
- ユーザーストーリー作成
- ユースケース図設計
- 受け入れ基準定義
- MoSCoW優先順位付け
- 要件のMECE分析

**スラッシュコマンド**:
- `/user-story` - ユーザーストーリー生成
- `/use-case` - ユースケース設計
- `/moscow` - MoSCoW優先順位付け

**使用例**:
```
@workspace /requirements-analyst
以下の機能のユーザーストーリーを作成してください。
機能: タスクの優先度変更
ユーザー: プロジェクトマネージャー
目的: タスクの重要度に応じて作業順序を調整する
受け入れ基準も含めてください。
```

---

### 10. Cloud Architect AI (`@workspace /cloud-architect`)

**専門領域**: クラウドアーキテクチャ設計

**主な機能**:
- AWS/Azure/GCP アーキテクチャ設計
- Well-Architected Framework適用
- コスト見積もり・最適化
- DR/BCP計画
- マルチクラウド戦略

**スラッシュコマンド**:
- `/aws` - AWS アーキテクチャ設計
- `/azure` - Azure アーキテクチャ設計
- `/gcp` - GCP アーキテクチャ設計
- `/cost` - コスト見積もり

**使用例**:
```
@workspace /cloud-architect
AWSで高可用性なWebアプリケーションインフラを設計してください。
要件:
- 可用性: 99.9%
- トラフィック: ピーク時5000req/sec
- リージョン: 東京、大阪のマルチAZ
- 月額予算: 50万円以内
Terraformコードも生成してください。
```

---

### 11. Project Manager AI (`@workspace /project-manager`)

**専門領域**: プロジェクト管理

**主な機能**:
- プロジェクト計画書作成
- WBS（作業分解構造）作成
- リスク管理表作成
- 工数見積もり（ストーリーポイント）
- スプリント計画

**スラッシュコマンド**:
- `/plan` - プロジェクト計画作成
- `/wbs` - WBS作成
- `/risks` - リスク分析
- `/estimate` - 工数見積もり

**使用例**:
```
@workspace /project-manager
以下のプロジェクトのWBSを作成してください。
プロジェクト: ECサイトリニューアル
期間: 3ヶ月
チーム: フロントエンド2名、バックエンド2名、デザイナー1名
主要機能: 商品検索改善、決済方法追加、レコメンド機能、管理画面刷新
```

---

### 12. Bug Hunter AI (`@workspace /bug-hunter`)

**専門領域**: バグ分析・デバッグ支援

**主な機能**:
- エラーログ・スタックトレース分析
- 根本原因分析（5 Whys）
- 再現手順の整理
- 修正パッチ提案
- 回帰テスト提案

**スラッシュコマンド**:
- `/analyze-error` - エラーログ分析
- `/stack-trace` - スタックトレース分析
- `/5whys` - 5 Whys分析
- `/fix` - 修正提案
- `/reproduce` - 再現手順作成

**使用例**:
```
@workspace /bug-hunter
以下のエラーを分析して根本原因を特定してください。

Error: TypeError: Cannot read property 'map' of undefined
    at UserList.render (UserList.jsx:42)
    at processChild (react-dom.js:1234)
```

---

### 13. Performance Optimizer AI (`@workspace /performance-optimizer`)

**専門領域**: パフォーマンス分析・最適化

**主な機能**:
- ボトルネック検出
- アルゴリズム計算量分析（Big-O）
- メモリ使用量最適化
- データベースクエリ最適化（N+1問題）
- キャッシング戦略提案

**スラッシュコマンド**:
- `/analyze` - パフォーマンス分析
- `/bottleneck` - ボトルネック検出
- `/optimize` - 最適化提案
- `/benchmark` - ベンチマーク計画
- `/cache` - キャッシング戦略

**使用例**:
```
@workspace /performance-optimizer
このループ処理を最適化してください。現在10秒かかっています。

for user in users:
    orders = db.query(Order).filter_by(user_id=user.id).all()
    for order in orders:
        process_order(order)
```

---

### 14. UI/UX Designer AI (`@workspace /uiux-designer`)

**専門領域**: UI/UX設計

**主な機能**:
- ワイヤーフレーム作成
- ユーザーフロー設計
- デザインシステム提案
- アクセシビリティチェック（WCAG 2.1）
- レスポンシブデザイン戦略

**スラッシュコマンド**:
- `/wireframe` - ワイヤーフレーム作成
- `/user-flow` - ユーザーフロー設計
- `/design-system` - デザインシステム提案
- `/a11y-check` - アクセシビリティチェック
- `/responsive` - レスポンシブデザイン提案

**使用例**:
```
@workspace /uiux-designer
タスク管理アプリのタスク一覧画面のワイヤーフレームを作成してください。
機能: タスクの絞り込み、ソート、ステータス変更、優先度表示
ターゲット: ビジネスユーザー、モバイル対応必須
```

---

### 15. Observability Engineer AI (`@workspace /observability-engineer`)

**専門領域**: 可観測性設計

**主な機能**:
- ログ・メトリクス・トレース設計（3本柱）
- SLI/SLO定義
- Grafanaダッシュボード設計
- アラートルール作成
- インシデント対応プロセス

**スラッシュコマンド**:
- `/sli-slo` - SLI/SLO定義
- `/logs` - ログ設計
- `/metrics` - メトリクス設計
- `/trace` - 分散トレーシング設計
- `/dashboard` - ダッシュボード設計
- `/alerts` - アラートルール作成

**使用例**:
```
@workspace /observability-engineer
Webアプリケーションの可観測性戦略を設計してください。
要件:
- サービス: FastAPI（Python）、PostgreSQL、Redis
- 目標: SLO 99.9%、p95レイテンシ < 200ms
- ツール: Prometheus、Grafana、Jaeger
```

---

### 16. Agile Coach AI (`@workspace /agile-coach`)

**専門領域**: アジャイル・スクラム支援

**主な機能**:
- スプリントプランニング
- レトロスペクティブ分析
- ベロシティ分析
- バーンダウンチャート生成
- ユーザーストーリー分割

**スラッシュコマンド**:
- `/sprint-plan` - スプリント計画作成
- `/retro` - レトロスペクティブ分析
- `/velocity` - ベロシティ分析
- `/burndown` - バーンダウンチャート生成
- `/split-story` - ユーザーストーリー分割
- `/standup` - デイリースタンドアップ議事録

**使用例**:
```
@workspace /agile-coach
スプリント3のレトロスペクティブを実施します。
Keep（続けること）:
- ペアプログラミングで品質向上
- デイリースタンドアップが効率的

Problem（問題）:
- テスト自動化が進まない
- ドキュメント不足

Try（試すこと）の提案をお願いします。
```

---

### 17. Quality Assurance AI (`@workspace /quality-assurance`)

**専門領域**: 品質保証・テスト戦略

**主な機能**:
- テスト戦略立案（テストピラミッド）
- 品質メトリクス定義
- 品質ゲート設計
- 欠陥密度分析
- 静的解析ツール設定

**スラッシュコマンド**:
- `/test-strategy` - テスト戦略立案
- `/metrics` - 品質メトリクス定義
- `/quality-gate` - 品質ゲート設計
- `/defect-analysis` - 欠陥分析
- `/static-analysis` - 静的解析設定

**使用例**:
```
@workspace /quality-assurance
新規プロジェクトのテスト戦略を立案してください。
プロジェクト: ECサイトリニューアル
技術スタック: React + TypeScript、Node.js、PostgreSQL
チーム: 開発5名、QA2名
目標品質: カバレッジ80%、バグ密度 < 1件/KLOC
```

---

## 実践例

### シナリオ1: 新規Webアプリケーションの開発

**ステップ1: 要件分析**
```
@workspace /requirements-analyst
以下のアプリケーションの要件を整理してください。
アプリケーション: タスク管理ツール
主な機能: タスクCRUD、プロジェクト管理、チーム共有、通知機能
```

**ステップ2: アーキテクチャ設計**
```
@workspace /system-architect
上記要件に基づいてアーキテクチャを設計してください。
C4モデルで可視化し、技術選定の理由も説明してください。
```

**ステップ3: データベース設計**
```
@workspace /database-schema-designer
アーキテクチャに基づいてデータベーススキーマを設計してください。
ER図とDDLを生成してください。
```

**ステップ4: API設計**
```
@workspace /api-designer
RESTful APIを設計してください。
OpenAPI仕様書を生成してください。
```

**ステップ5: CI/CD構築**
```
@workspace /devops-engineer
GitHub ActionsでCI/CDパイプラインを構築してください。
テスト、ビルド、デプロイを自動化してください。
```

**ステップ6: テスト戦略**
```
@workspace /test-engineer
包括的なテスト戦略を立ててください。
ユニット/統合/E2Eテストのカバレッジ目標も提示してください。
```

---

### シナリオ2: 既存コードのリファクタリング

**ステップ1: コードレビュー**
```
@workspace /code-reviewer
このモジュールをレビューしてください。
品質問題とセキュリティリスクを特定してください。
```

**ステップ2: セキュリティ監査**
```
@workspace /security-auditor
OWASP Top 10に基づいて詳細な脆弱性診断を実施してください。
```

**ステップ3: パフォーマンス改善**
```
@workspace /code-reviewer
/performance パフォーマンスボトルネックを検出して最適化案を提示してください。
```

**ステップ4: テストカバレッジ向上**
```
@workspace /test-engineer
/coverage 現在のカバレッジを分析して、不足しているテストを生成してください。
```

---

### シナリオ3: クラウド移行プロジェクト

**ステップ1: プロジェクト計画**
```
@workspace /project-manager
オンプレミスからAWSへの移行プロジェクト計画を立ててください。
期間: 6ヶ月、現行システム: 仮想マシン10台、データベース3台
```

**ステップ2: クラウドアーキテクチャ設計**
```
@workspace /cloud-architect
AWSでの移行先アーキテクチャを設計してください。
Well-Architected Frameworkに準拠し、コスト見積もりも提示してください。
```

**ステップ3: 移行戦略**
```
@workspace /cloud-architect
ゼロダウンタイムでの移行戦略を提案してください。
段階的移行のロードマップを作成してください。
```

**ステップ4: IaC化**
```
@workspace /devops-engineer
Terraformで全リソースをコード化してください。
dev/staging/prod環境を分離してください。
```

---

## トラブルシューティング

### エージェントが表示されない

**原因**: GitHub Copilot がエージェント定義を読み込んでいない

**解決策**:
1. エディタを再起動
2. `.github/agents/` ディレクトリが正しい場所にあるか確認
3. Markdownファイル（`.md`）のYAMLフロントマターの構文エラーをチェック

### エージェントの応答が期待と異なる

**原因**: プロンプトが不明確、または情報不足

**解決策**:
1. より具体的な質問をする
2. コンテキスト情報を追加（コード、要件など）
3. スラッシュコマンドを活用
4. 会話スターターを参考にする

### 生成された成果物の品質が低い

**原因**: 入力情報の不足、または要求が曖昧

**解決策**:
1. 詳細な要件を提示
2. 制約条件を明示（技術スタック、性能要件など）
3. 期待する出力形式を指定
4. 段階的に対話して詳細化

---

## FAQ

### Q1: 複数のエージェントを組み合わせて使えますか？

**A**: はい、推奨されます。例えば、`@workspace /system-architect`でアーキテクチャ設計後、`@workspace /database-schema-designer`でデータベース設計、`@workspace /api-designer`でAPI設計と段階的に進めることができます。

### Q2: 生成されたコードはそのまま使えますか？

**A**: 基本的には使えますが、プロジェクト固有の要件に応じて調整が必要な場合があります。特にセキュリティ関連のコードは必ずレビューしてください。

### Q3: エージェントをカスタマイズできますか？

**A**: はい、`.github/agents/` 内のMarkdownファイル（`.md`）を編集することでカスタマイズできます。各ファイルにはYAMLフロントマターとプロンプトが統合されています。

### Q4: 日本語と英語、どちらで質問すべきですか？

**A**: 全エージェントは日本語対応（`language: "ja"`）なので、日本語で質問してください。英語でも動作しますが、日本語の方が精度が高い場合があります。

### Q5: エージェントの応答が遅い場合は？

**A**: 複雑な質問や大量のコード分析には時間がかかります。質問を分割するか、スラッシュコマンドで範囲を絞ると改善する場合があります。

### Q6: 商用プロジェクトで使用できますか？

**A**: GitHub Copilot の利用規約に準じます。生成されたコードの著作権については、GitHub Copilot の規約を確認してください。

### Q7: 新しいエージェントを追加したい場合は？

**A**: `docs/agent-roadmap.md` に27種類のエージェントアイデアがあります。既存のエージェントをテンプレートとして新しいエージェントを作成できます。

### Q8: プライベートなコードをエージェントに渡しても安全ですか？

**A**: GitHub Copilot のプライバシーポリシーに準じます。機密情報は事前にマスキングすることを推奨します。

---

## 参考資料

- [GitHub Copilot 公式ドキュメント](https://docs.github.com/en/copilot)
- [エージェント開発ロードマップ](./agent-roadmap.md)
- [テンプレート集](../templates/)

---

## フィードバック・お問い合わせ

改善提案やバグ報告は、GitHubのIssueでお願いします。

---

**最終更新**: 2025-10-15
**バージョン**: 1.0.0
