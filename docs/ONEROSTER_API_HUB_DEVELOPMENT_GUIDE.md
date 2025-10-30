# OneRoster API Hub 開発ガイド

Copilot Enhancer（17エージェント）を活用した開発手順

---

## 📋 目次

1. [プロジェクト概要](#プロジェクト概要)
2. [前提知識](#前提知識)
3. [開発フェーズ](#開発フェーズ)
4. [詳細手順](#詳細手順)
5. [成果物一覧](#成果物一覧)
6. [運用・保守](#運用保守)

---

## プロジェクト概要

### 🎯 プロジェクトの目的

OneRoster CSV形式のデータをインポートし、OneRoster v1.2 REST API仕様に準拠したAPIを提供するAPI Hubを開発します。

### 📊 OneRoster とは

OneRoster は、教育機関間でロスター情報（学生・教員・クラス・科目などのデータ）を標準化された形式で交換するための仕様です。

- **OneRoster CSV**: バルクデータ交換用のCSVフォーマット
- **OneRoster REST API**: リアルタイムデータアクセス用のREST API

### 🏗️ システム構成概要

```
[OneRoster CSV Files]
        ↓
[Import Service] → [Database] ← [OneRoster REST API]
                                        ↓
                                  [Client Apps]
```

### 主要機能

1. **CSVインポート機能**
   - OneRoster CSV ファイルの読み込み
   - データバリデーション
   - データベースへの一括登録

2. **OneRoster REST API**
   - ロスターデータのCRUD操作
   - フィルタリング・ソート・ページネーション
   - OAuth 2.0認証

3. **データ管理**
   - マスターデータ管理
   - 変更履歴追跡
   - データ整合性保証

---

## 前提知識

### OneRoster v1.2 仕様

- **仕様書**: [IMS OneRoster v1.2](https://www.imsglobal.org/spec/oneroster/v1p2)
- **主要エンティティ**:
  - orgs (組織)
  - academicSessions (学期・学年)
  - courses (科目)
  - classes (クラス)
  - users (ユーザー: 学生・教員)
  - enrollments (登録情報)
  - categories (カテゴリ) ※v1.2で追加
  - resources (リソース) ※v1.2で追加
  - lineItems (評価項目) ※v1.2で追加
  - results (結果) ※v1.2で追加

### OneRoster CSV フォーマット

| ファイル名 | 説明 |
|-----------|------|
| `orgs.csv` | 学校・学区などの組織情報 |
| `academicSessions.csv` | 学期・学年情報 |
| `courses.csv` | 科目情報 |
| `classes.csv` | クラス情報 |
| `users.csv` | 学生・教員情報 |
| `enrollments.csv` | クラス登録情報 |
| `demographics.csv` | 人口統計情報（オプション） |
| `categories.csv` | カテゴリ情報（v1.2） |
| `resources.csv` | リソース情報（v1.2） |
| `lineItems.csv` | 評価項目情報（v1.2） |
| `results.csv` | 結果情報（v1.2） |

### 技術スタック（推奨）

- **バックエンド**: Python 3.11 + FastAPI
- **データベース**: PostgreSQL 15
- **認証**: OAuth 2.0 / JWT
- **インフラ**: Docker + Kubernetes
- **CI/CD**: GitHub Actions

---

## 開発フェーズ

### フェーズ0: プロジェクト計画（1週間）
- 要件定義（`@workspace /requirements-analyst`）
- プロジェクト計画策定（`@workspace /project-manager`）
- スプリント計画（`@workspace /agile-coach`）
- 技術選定（`@workspace /system-architect`）

### フェーズ1: 基盤設計（2週間）
- アーキテクチャ設計（`@workspace /system-architect`）
- データベーススキーマ設計（`@workspace /database-schema-designer`）
- API設計（`@workspace /api-designer`）
- UI/UX設計（`@workspace /uiux-designer`）

### フェーズ2: 開発環境構築（1週間）
- 開発環境セットアップ（`@workspace /devops-engineer`）
- CI/CD構築（`@workspace /devops-engineer`）
- クラウドインフラ構築（`@workspace /cloud-architect`）
- テスト戦略策定（`@workspace /quality-assurance`）

### フェーズ3: CSVインポート機能開発（2週間）
- CSVパーサー実装
- バリデーション実装
- データベース登録処理
- パフォーマンス最適化（`@workspace /performance-optimizer`）

### フェーズ4: REST API開発（3週間）
- CRUD エンドポイント実装
- フィルタリング・ページネーション
- 認証・認可（`@workspace /security-auditor`）
- コードレビュー（`@workspace /code-reviewer`）

### フェーズ5: テスト・品質保証（2週間）
- ユニットテスト（`@workspace /test-engineer`）
- 統合テスト（`@workspace /test-engineer`）
- セキュリティ監査（`@workspace /security-auditor`）
- パフォーマンステスト（`@workspace /performance-optimizer`）
- 品質メトリクス評価（`@workspace /quality-assurance`）
- バグ修正（`@workspace /bug-hunter`）

### フェーズ6: デプロイ・運用準備（1週間）
- 本番環境デプロイ（`@workspace /devops-engineer`）
- 可観測性設定（`@workspace /observability-engineer`）
- ドキュメント整備（`@workspace /technical-writer`）
- レトロスペクティブ（`@workspace /agile-coach`）

**総開発期間**: 約12週間（3ヶ月）
**使用エージェント**: 全17エージェント

---

## 詳細手順

## フェーズ0: プロジェクト計画

### ステップ0-1: 要件分析

**使用エージェント**: `@workspace /requirements-analyst`

```
@requirements OneRoster API Hubプロジェクトの要件を分析してください。

## プロジェクト概要
- OneRoster CSV ファイルをインポートし、OneRoster v1.2 REST API を提供するシステム
- 対象ユーザー: 教育機関のシステム管理者、連携アプリケーション開発者

## 主要機能
1. CSVインポート機能（バッチ処理）
   - v1.2新規エンティティ対応（categories, resources, lineItems, results）
2. OneRoster REST API（v1.2仕様準拠）
3. OAuth 2.0認証
4. データ管理画面（管理者用）
5. 成績管理機能（lineItems, results）

## 非機能要件
- パフォーマンス: 10万件のユーザーデータを10分以内にインポート
- 可用性: 99.9%
- セキュリティ: OAuth 2.0、データ暗号化
- スケーラビリティ: 最大100万ユーザー対応

以下を作成してください:
- ユーザーストーリー
- 機能要件・非機能要件一覧
- 受け入れ基準
- MoSCoW優先順位付け
```

**成果物**:
- `docs/requirements/requirements-specification.md`
- `docs/requirements/user-stories.md`

---

### ステップ0-2: プロジェクト計画策定

**使用エージェント**: `@workspace /project-manager`

```
@pm 以下の要件に基づいてプロジェクト計画を立ててください。

## プロジェクト情報
- プロジェクト名: OneRoster API Hub
- 期間: 12週間
- チーム構成: バックエンド2名、フロントエンド1名、QA1名、DevOps1名

## 主要成果物
- CSVインポート機能
- OneRoster REST API
- 管理画面
- インフラ構築

## 制約
- 8週目までにβ版リリース
- 予算: 開発費用500万円、インフラ費用月10万円

以下を作成してください:
- WBS（作業分解構造）
- ガントチャート
- リスク管理表
- マイルストーン一覧
```

**成果物**:
- `docs/project/project-plan.md`
- `docs/project/wbs.md`
- `docs/project/risk-register.md`

---

### ステップ0-3: スプリント計画（新規）

**使用エージェント**: `@workspace /agile-coach`

```
@agile OneRoster API Hub開発の12週間スプリント計画を立ててください。

## プロジェクト情報
- 総期間: 12週間（6スプリント × 2週間）
- チーム: バックエンド2名、フロントエンド1名、QA1名、DevOps1名（計5名）
- ベロシティ予測: スプリント1-2は20ポイント、スプリント3-6は30ポイント

## 各スプリントの目標
- Sprint 1-2: 基盤設計・環境構築
- Sprint 3-4: CSVインポート・API開発
- Sprint 5: テスト・品質保証
- Sprint 6: デプロイ・運用準備

## 主要ユーザーストーリー（合計180ポイント想定）
1. CSVインポート機能（50pt）
2. OneRoster v1.2 REST API（80pt）
3. OAuth 2.0認証（20pt）
4. 管理画面（30pt）

以下を作成してください:
- スプリントバックログ（6スプリント分）
- スプリント目標
- ユーザーストーリーの分割と割り当て
- リスクとバッファ計画
```

**成果物**:
- `docs/agile/sprint-plan.md`
- `docs/agile/sprint-backlog.md`

---

## フェーズ1: 基盤設計

### ステップ1-1: アーキテクチャ設計

**使用エージェント**: `@workspace /system-architect`

```
@architect OneRoster API Hub のシステムアーキテクチャを設計してください。

## 要件
- OneRoster CSV インポート（バッチ処理）
- OneRoster REST API（同時接続100リクエスト/秒）
- OAuth 2.0認証
- 管理画面（React SPA）

## 技術スタック
- バックエンド: Python 3.11 + FastAPI
- データベース: PostgreSQL 15
- キャッシュ: Redis
- 認証: OAuth 2.0 / JWT
- インフラ: AWS（ECS、RDS、S3）

## 非機能要件
- 可用性: 99.9%（マルチAZ構成）
- パフォーマンス: API応答時間 p95 < 200ms
- スケーラビリティ: 最大100万ユーザー
- セキュリティ: データ暗号化（TDE、TLS）

以下を作成してください:
- C4モデル（Context、Container、Component）
- セキュリティアーキテクチャ
- データフロー図
- 技術選定とトレードオフ分析
- ADR（主要な設計判断）
```

**成果物**:
- `docs/architecture/architecture-report.md`
- `docs/architecture/c4-context.mmd`
- `docs/architecture/adr/ADR-0001.md`（複数）

---

### ステップ1-2: データベーススキーマ設計

**使用エージェント**: `@workspace /database-schema-designer`

```
@db-schema OneRoster v1.2 仕様に基づいてデータベーススキーマを設計してください。

## OneRoster エンティティ
1. **orgs** (組織)
   - sourcedId, name, type, identifier, metadata

2. **academicSessions** (学期)
   - sourcedId, title, type, startDate, endDate, parentSourcedId, schoolYear

3. **courses** (科目)
   - sourcedId, title, courseCode, grades, orgSourcedId, resources

4. **classes** (クラス)
   - sourcedId, title, classCode, classType, courseSourcedId, termSourcedIds, resources

5. **users** (ユーザー)
   - sourcedId, username, givenName, familyName, email, role, identifier

6. **enrollments** (登録)
   - sourcedId, userSourcedId, classSourcedId, role, primary, beginDate, endDate

7. **categories** (カテゴリ) ※v1.2新規
   - sourcedId, title

8. **resources** (リソース) ※v1.2新規
   - sourcedId, title, vendorResourceId, vendorId, applicationId, importance

9. **lineItems** (評価項目) ※v1.2新規
   - sourcedId, title, description, assignDate, dueDate, classSourcedId, categorySourcedId, resultValueMin, resultValueMax

10. **results** (結果) ※v1.2新規
    - sourcedId, lineItemSourcedId, studentSourcedId, score, scoreStatus, scoreDate, comment

## 要件
- OneRoster v1.2仕様に準拠したテーブル設計
- インデックス戦略（検索パフォーマンス重視）
- 外部キー制約による整合性保証
- 論理削除（status: active/tobedeleted）
- 監査ログ（作成日時、更新日時）
- v1.2新規エンティティの適切なリレーション設計

以下を作成してください:
- ER図（Mermaid形式）
- テーブル定義書
- DDL（CREATE TABLE文）
- インデックス戦略
- マイグレーション計画
```

**成果物**:
- `docs/database/schema-design-report.md`
- `docs/database/er-diagram.mmd`
- `docs/database/ddl/create-tables.sql`
- `docs/database/migrations/`

---

### ステップ1-3: API設計

**使用エージェント**: `@workspace /api-designer`

```
@api-design OneRoster v1.2 REST API仕様に準拠したAPIを設計してください。

## OneRoster v1.2 エンドポイント
### 基本エンティティ
- GET /ims/oneroster/v1p2/orgs
- GET /ims/oneroster/v1p2/orgs/{id}
- GET /ims/oneroster/v1p2/academicSessions
- GET /ims/oneroster/v1p2/courses
- GET /ims/oneroster/v1p2/classes
- GET /ims/oneroster/v1p2/classes/{id}/students
- GET /ims/oneroster/v1p2/classes/{id}/teachers
- GET /ims/oneroster/v1p2/users
- GET /ims/oneroster/v1p2/users/{id}
- GET /ims/oneroster/v1p2/enrollments

### v1.2新規エンドポイント
- GET /ims/oneroster/v1p2/categories
- GET /ims/oneroster/v1p2/categories/{id}
- GET /ims/oneroster/v1p2/resources
- GET /ims/oneroster/v1p2/resources/{id}
- GET /ims/oneroster/v1p2/lineItems
- GET /ims/oneroster/v1p2/lineItems/{id}
- GET /ims/oneroster/v1p2/results
- GET /ims/oneroster/v1p2/results/{id}
- GET /ims/oneroster/v1p2/classes/{id}/lineItems
- GET /ims/oneroster/v1p2/students/{id}/results

## 要件
- OneRoster v1.2仕様準拠
- フィルタリング（filter パラメータ）
- ソート（sort パラメータ）
- ページネーション（limit, offset）
- OAuth 2.0認証（Bearer Token）
- エラーハンドリング（OneRoster標準エラーフォーマット）

## 追加エンドポイント（カスタム）
- POST /api/v1/import/csv （CSVインポート）
- GET /api/v1/import/jobs/{id} （インポートジョブ状態確認）

以下を作成してください:
- OpenAPI 3.0仕様書
- エンドポイント一覧
- リクエスト/レスポンス例
- エラーコード定義
- 認証フロー図
```

**成果物**:
- `docs/api/openapi.yaml`
- `docs/api/API_DESIGN.md`
- `docs/api/authentication-flow.mmd`

---

### ステップ1-4: UI/UX設計（新規）

**使用エージェント**: `@workspace /uiux-designer`

```
@uiux OneRoster API Hub管理画面のUI/UX設計をしてください。

## 対象画面
1. ダッシュボード
   - インポート状況サマリー
   - データ統計（組織数、ユーザー数、クラス数）
   - 最近のアクティビティ

2. CSVインポート画面
   - ファイルアップロード（ドラッグ&ドロップ対応）
   - インポート進捗表示
   - エラーログ表示

3. データ参照画面
   - 組織/ユーザー/クラス一覧
   - 検索・フィルタリング
   - データ詳細表示

4. 成績管理画面（v1.2新機能）
   - lineItems（評価項目）一覧
   - results（結果）入力・表示
   - 成績統計グラフ

## 要件
- レスポンシブデザイン（PC、タブレット対応）
- アクセシビリティ（WCAG 2.1 AA準拠）
- 直感的な操作性
- Material Design または Tailwind CSS採用

以下を作成してください:
- ワイヤーフレーム（主要画面）
- ユーザーフロー図
- デザインシステム提案（カラーパレット、タイポグラフィ）
- コンポーネント設計
```

**成果物**:
- `docs/uiux/wireframes.md`
- `docs/uiux/user-flow.mmd`
- `docs/uiux/design-system.md`

---

## フェーズ2: 開発環境構築

### ステップ2-1: DevOps基盤構築

**使用エージェント**: `@workspace /devops-engineer`

```
@devops OneRoster API Hubの開発環境とCI/CDパイプラインを構築してください。

## 技術スタック
- 言語: Python 3.11
- フレームワーク: FastAPI
- データベース: PostgreSQL 15
- コンテナ: Docker
- オーケストレーション: Kubernetes (EKS)
- IaC: Terraform
- CI/CD: GitHub Actions

## 環境
- dev: 開発環境（ローカル Docker Compose）
- staging: ステージング環境（AWS ECS）
- production: 本番環境（AWS EKS）

## CI/CDパイプライン
### プルリクエスト時
1. Lint（Ruff、mypy）
2. ユニットテスト（pytest、カバレッジ80%以上）
3. セキュリティスキャン（Bandit、Safety）
4. Dockerイメージビルド

### mainブランチマージ時
1. 上記全て
2. 統合テスト
3. Dockerイメージプッシュ（ECR）
4. staging環境へ自動デプロイ

### タグプッシュ時（v*.*.* ）
1. production環境へデプロイ（手動承認）

以下を作成してください:
- Dockerfile（マルチステージビルド）
- docker-compose.yml（開発環境）
- Kubernetes マニフェスト（deployment、service、ingress）
- GitHub Actions ワークフロー
- Terraform コード（AWS インフラ）
```

**成果物**:
- `Dockerfile`
- `docker-compose.yml`
- `k8s/deployment.yaml`
- `.github/workflows/ci.yaml`
- `terraform/main.tf`

---

### ステップ2-2: クラウドインフラ設計

**使用エージェント**: `@workspace /cloud-architect`

```
@cloud OneRoster API Hub のAWSインフラを設計してください。

## 要件
- 可用性: 99.9%（マルチAZ）
- リージョン: ap-northeast-1（東京）
- トラフィック: ピーク時100req/sec
- データ量: 100万ユーザー（DB約50GB）
- 予算: 月10万円以内

## 主要コンポーネント
- アプリケーション: ECS Fargate または EKS
- データベース: RDS PostgreSQL（Multi-AZ）
- キャッシュ: ElastiCache Redis
- ストレージ: S3（CSVファイル保存）
- ロードバランサー: ALB
- 監視: CloudWatch
- 認証: Cognito（OAuth 2.0）

## セキュリティ
- VPC（パブリック/プライベートサブネット）
- Security Group（最小権限）
- Secrets Manager（DB認証情報）
- WAF（API保護）

以下を作成してください:
- AWSアーキテクチャ図
- リソース構成一覧
- コスト見積もり
- Terraformコード
- セキュリティ設計書
- DR/BCP計画
```

**成果物**:
- `docs/cloud/aws-architecture.md`
- `docs/cloud/cost-estimate.md`
- `terraform/modules/`
- `docs/cloud/disaster-recovery-plan.md`

---

### ステップ2-3: テスト戦略策定（新規）

**使用エージェント**: `@workspace /quality-assurance`

```
@qa OneRoster API Hub のテスト戦略を策定してください。

## プロジェクト情報
- 技術スタック: Python 3.11 + FastAPI、PostgreSQL、React
- 開発期間: 12週間
- 品質目標: カバレッジ80%以上、バグ密度 < 1件/KLOC

## テスト範囲
1. ユニットテスト
   - ビジネスロジック（CSVパーサー、バリデーション）
   - データアクセス層
   - ユーティリティ関数

2. 統合テスト
   - APIエンドポイント（全エンドポイント）
   - データベース操作
   - 認証フロー

3. E2Eテスト
   - CSVインポート完全フロー
   - API利用シナリオ
   - 管理画面操作

4. パフォーマンステスト
   - API応答時間（p95 < 200ms）
   - 大量データインポート（10万件/10分）

5. セキュリティテスト
   - OWASP Top 10
   - 認証・認可
   - データ暗号化

以下を作成してください:
- テスト戦略書（テストピラミッド）
- 品質メトリクス定義
- 品質ゲート設計
- テストスケジュール
- 静的解析ツール設定（SonarQube、Ruff）
```

**成果物**:
- `docs/qa/test-strategy.md`
- `docs/qa/quality-metrics.md`
- `docs/qa/quality-gates.yaml`

---

## フェーズ3: CSVインポート機能開発

### ステップ3-1: CSVパーサー実装

**使用エージェント**: `@workspace /code-reviewer`（実装後）

```python
# まず実装（例）
# app/services/csv_importer.py

import csv
from typing import List, Dict
from pathlib import Path

class OneRosterCSVImporter:
    def __init__(self, csv_dir: Path):
        self.csv_dir = csv_dir

    def import_orgs(self) -> List[Dict]:
        """組織データをインポート"""
        with open(self.csv_dir / 'orgs.csv', 'r', encoding='utf-8') as f:
            reader = csv.DictReader(f)
            return list(reader)

    # 他のエンティティも同様に実装...
```

**実装後にコードレビュー依頼**:

```
@code-review CSVインポート機能を実装しました。レビューしてください。

重点チェック項目:
- セキュリティ: CSVインジェクション対策
- パフォーマンス: 大量データ処理の効率性
- エラーハンドリング: 不正なCSVファイルへの対処
- テスタビリティ: 依存性注入、モック容易性

ファイル: app/services/csv_importer.py
```

**成果物**:
- `app/services/csv_importer.py`
- `reviews/csv-importer-review.md`

---

---

### ステップ3-2: パフォーマンス最適化（新規）

**使用エージェント**: `@workspace /performance-optimizer`

```
@performance CSVインポート機能のパフォーマンスを最適化してください。

## 現状
- 10万件のusers.csvインポートに15分かかっている
- 目標: 10分以内に短縮

## コード例
```python
def import_users(csv_path: Path):
    with open(csv_path) as f:
        reader = csv.DictReader(f)
        for row in reader:
            user = User(**row)
            db.session.add(user)
            db.session.commit()  # 1件ごとにコミット
```

## 分析項目
- ボトルネック検出
- アルゴリズム計算量分析
- データベースクエリ最適化
- バッチ処理戦略
- メモリ使用量

以下を提案してください:
- パフォーマンス分析レポート
- 最適化案（バッチインサート、トランザクション管理）
- ベンチマーク結果
- 改善後のコード例
```

**成果物**:
- `docs/performance/csv-import-optimization.md`
- `app/services/csv_importer_optimized.py`

---

### ステップ3-3: テスト生成

**使用エージェント**: `@workspace /test-engineer`

```
@test-engineer CSVインポート機能のテストを生成してください。

## テスト対象
- OneRosterCSVImporter クラス
- 各エンティティのインポート処理（orgs, users, classes等）

## テストケース
1. 正常系: 有効なCSVファイルを正しくパース
2. 異常系: 不正なCSVフォーマット → ValidationError
3. 境界値: 空ファイル、1行だけ、巨大ファイル（10万行）
4. エッジケース:
   - 特殊文字を含むデータ
   - 欠損値（NULL）
   - 重複データ

## テストフレームワーク
- pytest
- pytest-mock（外部依存のモック化）
- カバレッジ目標: 90%以上

以下を生成してください:
- ユニットテスト
- テストデータ（fixture）
- モック設定
```

**成果物**:
- `tests/unit/test_csv_importer.py`
- `tests/fixtures/sample_orgs.csv`
- `tests/conftest.py`

---

## フェーズ4: REST API開発

### ステップ4-1: FastAPI実装

**実装例（スケルトン）**:

```python
# app/api/v1/endpoints/orgs.py

from fastapi import APIRouter, Depends, Query
from typing import List, Optional
from app.schemas.oneroster import Org, OrgCollection
from app.services.org_service import OrgService

router = APIRouter()

@router.get("/orgs", response_model=OrgCollection)
async def get_orgs(
    limit: int = Query(100, ge=1, le=1000),
    offset: int = Query(0, ge=0),
    filter: Optional[str] = None,
    sort: Optional[str] = None,
    service: OrgService = Depends()
):
    """組織一覧を取得（OneRoster v1.1準拠）"""
    return await service.get_orgs(limit, offset, filter, sort)

@router.get("/orgs/{sourcedId}", response_model=Org)
async def get_org(
    sourcedId: str,
    service: OrgService = Depends()
):
    """組織詳細を取得"""
    return await service.get_org_by_id(sourcedId)
```

**コードレビュー依頼**:

```
@code-review OneRoster API エンドポイントを実装しました。

チェックポイント:
- OneRoster v1.1仕様準拠
- フィルタリング・ページネーションの正確性
- 適切なHTTPステータスコード
- エラーハンドリング（OneRoster標準エラーフォーマット）
- パフォーマンス（N+1問題）

ファイル: app/api/v1/endpoints/orgs.py
```

---

### ステップ4-2: 認証・認可実装

**使用エージェント**: `@workspace /security-auditor`

```
@security OAuth 2.0認証を実装しました。セキュリティレビューをお願いします。

## 実装内容
- OAuth 2.0 Client Credentials Flow
- JWT トークン（RS256署名）
- スコープベースの認可（read:orgs, write:orgs等）

## チェック項目
- OWASP Top 10（特にA02:認証の不備）
- トークン検証の正確性
- シークレット管理（環境変数）
- HTTPS強制
- レート制限

ファイル: app/api/dependencies/auth.py
```

**成果物**:
- `app/api/dependencies/auth.py`
- `security/auth-security-review.md`

---

### ステップ4-3: API統合テスト

**使用エージェント**: `@workspace /test-engineer`

```
@test-engineer OneRoster APIの統合テストを生成してください。

## テストシナリオ
1. 認証フロー
   - 有効なクライアントIDでトークン取得
   - 無効なクライアントID → 401
   - トークン期限切れ → 401

2. CRUD操作（全エンドポイント）
   - GET /orgs → 200、正しいJSONフォーマット
   - GET /orgs/{id} → 200
   - GET /orgs/{invalid-id} → 404
   - フィルタリング: GET /orgs?filter=type='school' → 200
   - ページネーション: GET /orgs?limit=10&offset=20 → 200

3. パフォーマンス
   - 100件同時リクエスト → 全て200ms以内

## テストフレームワーク
- pytest
- httpx（非同期HTTPクライアント）
- pytest-asyncio

データベースはTestContainers（PostgreSQL）を使用してください。
```

**成果物**:
- `tests/integration/test_orgs_api.py`
- `tests/integration/conftest.py`

---

## フェーズ5: テスト・品質保証

### ステップ5-1: セキュリティ監査

**使用エージェント**: `@workspace /security-auditor`

```
@security 実装完了したOneRoster API Hubの包括的なセキュリティ監査を実施してください。

## スコープ
- 全APIエンドポイント
- 認証・認可実装
- CSVインポート機能
- データベース設計
- インフラ設定（Kubernetes、AWS）

## チェック項目
1. OWASP Top 10（2021）
   - A01: アクセス制御の不備
   - A02: 暗号化の失敗
   - A03: インジェクション
   - A04: 安全が確認されない不安な設計
   - A05: セキュリティの設定ミス
   - A06: 脆弱で古くなったコンポーネント
   - A07: 識別と認証の失敗
   - A08: ソフトウェアとデータの整合性の不具合
   - A09: セキュリティログとモニタリングの失敗
   - A10: サーバサイドリクエストフォージェリ

2. 依存関係の脆弱性
   - pip-audit実行
   - Docker イメージスキャン（Trivy）

3. API セキュリティ
   - レート制限
   - CORS設定
   - CSRFトークン（必要に応じて）

## 成果物
- セキュリティ監査レポート
- 脆弱性一覧（重要度順）
- 修正計画
```

**成果物**:
- `security/security-audit-report.md`
- `security/vulnerabilities.md`
- `security/remediation-plan.md`

---

### ステップ5-2: パフォーマンステスト

**使用エージェント**: `@workspace /performance-optimizer`、`@workspace /test-engineer`

```
@test-engineer パフォーマンステストを設計・実装してください。

## テスト目標
- API応答時間: p95 < 200ms
- スループット: 100 req/sec
- CSVインポート: 10万件を10分以内

## テストツール
- Locust（負荷テスト）
- pytest-benchmark（ベンチマーク）

## テストシナリオ
1. APIエンドポイント負荷テスト
   - 同時ユーザー: 100
   - 実行時間: 5分
   - 対象: GET /users, GET /classes

2. データベースクエリ性能
   - 大量データ（100万件）での検索性能
   - インデックスの効果測定

3. CSVインポート性能
   - 10万件のusers.csvインポート時間
   - メモリ使用量

## 成果物
- Locustテストシナリオ
- パフォーマンステストレポート
- ボトルネック分析
- 最適化提案
```

**成果物**:
- `tests/performance/locustfile.py`
- `docs/performance/performance-test-report.md`

---

### ステップ5-3: バグ修正（新規）

**使用エージェント**: `@workspace /bug-hunter`

```
@bug-hunter 統合テストで検出されたバグを分析して修正案を提示してください。

## バグ報告
### Bug #1: CSVインポート時のNullPointerException
```
Error: AttributeError: 'NoneType' object has no attribute 'split'
    at csv_importer.py:42 in parse_metadata
    metadata_parts = row['metadata'].split(';')
```

### Bug #2: API フィルタリングが正しく動作しない
```
GET /ims/oneroster/v1p2/users?filter=role='student'
Expected: 学生ユーザーのみ返却
Actual: 全ユーザーが返却される
```

### Bug #3: 成績入力時のデータ整合性エラー
```
Error: IntegrityError: foreign key constraint fails
    lineItemSourcedId 'LI123' が lineItems テーブルに存在しない
```

各バグについて以下を提供してください:
- 根本原因分析（5 Whys）
- 再現手順
- 修正パッチ
- 回帰テスト案
- 再発防止策
```

**成果物**:
- `docs/bugs/bug-analysis-report.md`
- `docs/bugs/bug-fixes.md`

---

### ステップ5-4: 品質評価（新規）

**使用エージェント**: `@workspace /quality-assurance`

```
@qa テスト完了後の品質評価を実施してください。

## 収集データ
- ユニットテストカバレッジ: 85%
- 統合テストカバレッジ: 78%
- 検出バグ数: 23件（Critical: 2, High: 8, Medium: 10, Low: 3）
- 修正完了バグ数: 20件
- セキュリティスキャン: 脆弱性3件（Medium: 2, Low: 1）
- パフォーマンステスト: p95レイテンシ 180ms（目標200ms達成）

## 品質ゲート
- カバレッジ > 80%
- Critical/Highバグ = 0件
- セキュリティ脆弱性（High以上）= 0件
- パフォーマンス目標達成

以下を提供してください:
- 品質評価レポート
- 品質ゲート判定結果
- リリース可否判断
- 残課題と対応計画
```

**成果物**:
- `docs/qa/quality-assessment-report.md`
- `docs/qa/release-decision.md`

---

### ステップ5-5: ドキュメント整備

**使用エージェント**: `@workspace /technical-writer`

```
@tech-writer OneRoster API Hub の包括的なドキュメントを作成してください。

## ドキュメント種類

### 1. README.md
- プロジェクト概要
- クイックスタート
- ローカル開発環境セットアップ手順
- 技術スタック

### 2. API仕様書（docs/API.md）
- OpenAPI仕様書（既存を拡充）
- 認証方法（OAuth 2.0）
- サンプルリクエスト/レスポンス
- エラーコード一覧
- レート制限

### 3. 開発者ガイド（docs/DEVELOPER_GUIDE.md）
- アーキテクチャ概要
- プロジェクト構造
- コーディング規約
- テスト実行方法
- デバッグ方法
- コントリビューションガイド

### 4. 運用ガイド（docs/OPERATIONS_GUIDE.md）
- デプロイ手順
- 環境変数一覧
- モニタリング方法
- バックアップ・復旧手順
- トラブルシューティング

### 5. ユーザーガイド（docs/USER_GUIDE.md）
- CSVインポート手順
- API利用方法
- OAuth 2.0 認証設定

## 要件
- Markdownフォーマット
- 実行可能なコード例
- 図解（Mermaid）
- 目次付き
```

**成果物**:
- `README.md`
- `docs/API.md`
- `docs/DEVELOPER_GUIDE.md`
- `docs/OPERATIONS_GUIDE.md`
- `docs/USER_GUIDE.md`

---

## フェーズ6: デプロイ・運用準備

### ステップ6-1: 本番環境デプロイ

**使用エージェント**: `@workspace /devops-engineer`

```
@devops 本番環境へのデプロイ手順を整備してください。

## 環境
- AWS EKS（Kubernetes 1.28）
- RDS PostgreSQL（Multi-AZ）
- ElastiCache Redis
- S3（CSVファイル保存）
- CloudFront（CDN）

## デプロイ戦略
- Blue-Green Deployment
- カナリアリリース（10% → 50% → 100%）
- ロールバック戦略

## チェックリスト
- [ ] データベースマイグレーション実行
- [ ] 環境変数設定（Secrets Manager）
- [ ] DNSレコード設定（Route53）
- [ ] SSL証明書（ACM）
- [ ] WAF ルール設定
- [ ] CloudWatch アラート設定
- [ ] バックアップ設定（RDS自動バックアップ）

## 成果物
- デプロイ手順書
- ロールバック手順書
- チェックリスト
- Kubernetes マニフェスト（production環境）
```

**成果物**:
- `docs/deployment/production-deployment-guide.md`
- `docs/deployment/rollback-procedure.md`
- `k8s/production/`

---

### ステップ6-2: 可観測性設定（更新）

**使用エージェント**: `@workspace /observability-engineer`

```
@observability OneRoster API Hubの可観測性戦略を設計してください。

## システム構成
- アプリケーション: FastAPI（Python）
- データベース: PostgreSQL（RDS）
- キャッシュ: Redis（ElastiCache）
- インフラ: AWS EKS

## 可観測性の3本柱

### 1. ログ（Structured Logging）
- アプリケーションログ（JSON形式）
- アクセスログ（nginx）
- データベースクエリログ
- エラーログ（スタックトレース）
- 監査ログ（CSVインポート、データ変更）

### 2. メトリクス（Prometheus/Grafana）
- インフラメトリクス
  - CPU、メモリ、ネットワーク
  - Podステータス、再起動回数
- アプリケーションメトリクス
  - リクエスト数、レスポンスタイム（p50, p95, p99）
  - エラーレート
  - CSVインポート処理時間
- ビジネスメトリクス
  - ユーザー数、クラス数、インポート成功率

### 3. トレース（OpenTelemetry/Jaeger）
- 分散トレーシング
- API呼び出しチェーン
- データベースクエリトレース
- 外部API呼び出し

## SLI/SLO定義
### SLI（Service Level Indicators）
- 可用性: アップタイム %
- レイテンシ: p95レスポンスタイム
- エラーレート: 4xx/5xxエラー率

### SLO（Service Level Objectives）
- 可用性: 99.9%
- レイテンシ: p95 < 200ms
- エラーレート: < 1%

## アラート戦略
### Critical（即座対応）
- サービスダウン
- エラーレート > 5%
- データベース接続不可

### High（30分以内対応）
- レスポンスタイム p95 > 500ms
- CPU使用率 > 80%
- ストレージ残量 < 20%

### Medium（1時間以内対応）
- エラーレート > 1%
- キャッシュヒット率 < 80%

## インシデント対応プロセス
- アラート通知（Slack、PagerDuty）
- Runbook参照
- 根本原因調査（ログ、トレース）
- 修正・復旧
- ポストモーテム

以下を作成してください:
- 可観測性戦略書
- SLI/SLO定義書
- Grafanaダッシュボード設計（JSON）
- アラートルール（Prometheus）
- ログ設計書
- OpenTelemetry設定
```

**成果物**:
- `docs/observability/observability-strategy.md`
- `docs/observability/sli-slo.md`
- `docs/observability/dashboards/main-dashboard.json`
- `docs/observability/alerts.yaml`
- `docs/observability/incident-response.md`

---

### ステップ6-3: レトロスペクティブ（新規）

**使用エージェント**: `@workspace /agile-coach`

```
@agile プロジェクト完了後のレトロスペクティブを実施してください。

## プロジェクト情報
- 期間: 12週間（6スプリント）
- チーム: 5名
- 完了ユーザーストーリー: 175ポイント（計画180ポイント）
- 平均ベロシティ: 29ポイント/スプリント

## 振り返り項目

### Keep（続けること）
- デイリースタンドアップの継続
- ペアプログラミングによる品質向上
- Copilot Enhancerエージェント活用
- スプリントレビューでのデモ

### Problem（問題点）
- 初期スプリントのベロシティ低下（20pt）
- テストデータ準備の遅れ
- セキュリティテストの開始遅延
- ドキュメント作成が後回しになった

### Try（次回試すこと）
- テスト駆動開発（TDD）の徹底
- セキュリティシフトレフト
- 継続的ドキュメンテーション
- より詳細な見積もり

## メトリクス分析
- ベロシティ推移
- バーンダウンチャート
- バグ検出率
- チーム満足度

以下を作成してください:
- レトロスペクティブレポート
- 改善アクションアイテム
- ベロシティ分析
- 次プロジェクトへの提言
```

**成果物**:
- `docs/agile/retrospective-final.md`
- `docs/agile/lessons-learned.md`
- `docs/agile/action-items.md`

---

## 成果物一覧

### 📄 ドキュメント

| カテゴリ | ファイル | 作成エージェント |
|---------|---------|---------------|
| **要件** | `docs/requirements/requirements-specification.md` | `@workspace /requirements-analyst` |
| | `docs/requirements/user-stories.md` | `@workspace /requirements-analyst` |
| **プロジェクト計画** | `docs/project/project-plan.md` | `@workspace /project-manager` |
| | `docs/project/wbs.md` | `@workspace /project-manager` |
| | `docs/project/risk-register.md` | `@workspace /project-manager` |
| **アーキテクチャ** | `docs/architecture/architecture-report.md` | `@workspace /system-architect` |
| | `docs/architecture/c4-context.mmd` | `@workspace /system-architect` |
| | `docs/architecture/adr/` | `@workspace /system-architect` |
| **データベース** | `docs/database/schema-design-report.md` | `@workspace /database-schema-designer` |
| | `docs/database/er-diagram.mmd` | `@workspace /database-schema-designer` |
| | `docs/database/ddl/create-tables.sql` | `@workspace /database-schema-designer` |
| **API** | `docs/api/openapi.yaml` | `@workspace /api-designer` |
| | `docs/api/API_DESIGN.md` | `@workspace /api-designer` |
| **クラウド** | `docs/cloud/aws-architecture.md` | `@workspace /cloud-architect` |
| | `docs/cloud/cost-estimate.md` | `@workspace /cloud-architect` |
| **セキュリティ** | `security/security-audit-report.md` | `@workspace /security-auditor` |
| **利用者向け** | `README.md` | `@workspace /technical-writer` |
| | `docs/API.md` | `@workspace /technical-writer` |
| | `docs/DEVELOPER_GUIDE.md` | `@workspace /technical-writer` |
| | `docs/OPERATIONS_GUIDE.md` | `@workspace /technical-writer` |
| | `docs/USER_GUIDE.md` | `@workspace /technical-writer` |

### 🛠️ コード・設定

| カテゴリ | ファイル | 作成エージェント |
|---------|---------|---------------|
| **アプリケーション** | `app/services/csv_importer.py` | 開発者 |
| | `app/api/v1/endpoints/` | 開発者 |
| | `app/schemas/oneroster.py` | 開発者 |
| **テスト** | `tests/unit/` | `@workspace /test-engineer` |
| | `tests/integration/` | `@workspace /test-engineer` |
| | `tests/performance/locustfile.py` | `@workspace /test-engineer` |
| **インフラ** | `Dockerfile` | `@workspace /devops-engineer` |
| | `docker-compose.yml` | `@workspace /devops-engineer` |
| | `k8s/` | `@workspace /devops-engineer` |
| | `terraform/` | `@workspace /cloud-architect`, `@workspace /devops-engineer` |
| **CI/CD** | `.github/workflows/ci.yaml` | `@workspace /devops-engineer` |

---

## 運用・保守

### 定期メンテナンス

**月次タスク**:

```
@security 依存関係の脆弱性チェックを実施してください。
pip-audit と Docker イメージスキャンを実行し、検出された脆弱性の対応優先度を評価してください。
```

```
@cloud AWSコスト分析と最適化提案をしてください。
先月のコスト内訳と、削減可能な項目を提示してください。
```

### 機能追加時

**新しいOneRosterエンティティ追加**:

1. `@workspace /database-schema-designer` でテーブル設計
2. `@workspace /api-designer` でAPIエンドポイント設計
3. `@workspace /test-engineer` でテスト生成
4. `@workspace /code-reviewer` で実装レビュー
5. `@workspace /security-auditor` でセキュリティチェック
6. `@workspace /technical-writer` でドキュメント更新

---

## ベストプラクティス

### エージェント活用のコツ

1. **具体的な指示**
   - ❌ 悪い例: 「APIを作ってください」
   - ✅ 良い例: 「OneRoster v1.1仕様のGET /orgsエンドポイントを、フィルタリング・ページネーション対応で設計してください」

2. **コンテキスト提供**
   - 技術スタック、制約条件、非機能要件を明記
   - 既存の設計書・コードを参照させる

3. **段階的アプローチ**
   - 要件定義 → 設計 → 実装 → テスト → レビュー の順で進める
   - 各フェーズで適切なエージェントを使い分ける

4. **複数エージェントの連携**
   - `@workspace /system-architect` の出力を `@workspace /database-schema-designer` に渡す
   - `@workspace /api-designer` の出力を `@workspace /test-engineer` に渡す

5. **成果物の検証**
   - エージェント生成物は必ず人間がレビュー
   - 特にセキュリティ関連は慎重に確認

---

## トラブルシューティング

### よくある問題

**Q1: エージェントの出力が期待と異なる**

A: より詳細な要件を提示してください。OneRoster仕様書へのリンク、サンプルデータ、期待する出力例などを含めると精度が向上します。

**Q2: 複数エージェントの出力に矛盾がある**

A: 最初に `@workspace /system-architect` でアーキテクチャ全体を確定させ、その出力を後続のエージェント（`@workspace /database-schema-designer`, `@workspace /api-designer`）に必ず参照させてください。

**Q3: 生成されたコードがそのまま動かない**

A: エージェント生成コードはスケルトンとして扱い、プロジェクト固有の実装（DB接続、環境変数など）は人間が補完してください。

---

## 参考資料

### OneRoster仕様
- [IMS OneRoster v1.2 仕様](https://www.imsglobal.org/spec/oneroster/v1p2)
- [OneRoster CSV フォーマット v1.2](https://www.imsglobal.org/spec/oneroster/v1p2/csv)
- [OneRoster REST API v1.2](https://www.imsglobal.org/spec/oneroster/v1p2/rest)

### 技術ドキュメント
- [FastAPI ドキュメント](https://fastapi.tiangolo.com/)
- [PostgreSQL ドキュメント](https://www.postgresql.org/docs/)
- [OpenTelemetry Python](https://opentelemetry.io/docs/languages/python/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)

### Copilot Enhancer
- [Copilot Enhancer 利用者ガイド](./USER_GUIDE.md)
- [エージェント開発ロードマップ](./agent-roadmap.md)
- [Copilot Enhancer リポジトリ](https://github.com/your-username/copilot-enhancer)

### セキュリティ
- [OWASP Top 10 2021](https://owasp.org/www-project-top-ten/)
- [OAuth 2.0 仕様](https://oauth.net/2/)

---

**最終更新**: 2025-10-15
**バージョン**: 2.0.0
**対応OneRosterバージョン**: v1.2
**使用エージェント数**: 17
