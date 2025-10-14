# データベースマイグレーション計画

## マイグレーション情報

- **マイグレーション番号**: [001]
- **作成日**: [YYYY-MM-DD]
- **作成者**: [Name]
- **実施予定日**: [YYYY-MM-DD]
- **影響範囲**: [新規テーブル作成 / 既存テーブル変更 / データ移行]

---

## 1. 変更概要

### 変更の目的
[このマイグレーションの目的とビジネス価値を記述]

### 変更内容サマリー
- [ ] 新規テーブル作成: [table_name]
- [ ] カラム追加: [table_name.column_name]
- [ ] インデックス追加: [index_name]
- [ ] データ移行: [既存データの変換・移行]
- [ ] 制約追加: [constraint_name]

---

## 2. 影響分析

### 影響を受けるシステム
- [ ] アプリケーション: [影響範囲の説明]
- [ ] バッチ処理: [影響範囲の説明]
- [ ] レポート機能: [影響範囲の説明]
- [ ] 外部システム連携: [影響範囲の説明]

### パフォーマンスへの影響
- **想定実行時間**: [5分 / 30分 / 2時間]
- **テーブルロック**: [あり / なし]
- **ダウンタイム**: [必要 / 不要]
- **データベースサイズ増加**: [+10GB]

---

## 3. 前提条件・依存関係

### 実施前の前提条件
- [ ] マイグレーション#XXXが完了していること
- [ ] データベースバックアップが取得されていること
- [ ] アプリケーションの特定バージョンがデプロイされていること
- [ ] メンテナンスウィンドウが確保されていること

### 依存する他のマイグレーション
- マイグレーション#XXX: [依存関係の説明]

---

## 4. マイグレーションスクリプト（Forward）

### 4.1 DDL変更

```sql
-- ============================================
-- Migration #001: [変更内容のタイトル]
-- Date: YYYY-MM-DD
-- ============================================

-- トランザクション開始（DDLがトランザクション対応のDBの場合）
START TRANSACTION;

-- 新規テーブル作成
CREATE TABLE new_table (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- 既存テーブルへのカラム追加
ALTER TABLE existing_table
    ADD COLUMN new_column VARCHAR(100) NULL AFTER some_column,
    ADD INDEX idx_new_column (new_column);

-- 制約追加
ALTER TABLE existing_table
    ADD CONSTRAINT fk_new_relation
    FOREIGN KEY (new_column_id) REFERENCES other_table(id)
    ON DELETE CASCADE;

-- コミット
COMMIT;
```

### 4.2 データ移行

```sql
-- データ移行スクリプト
-- 既存データを新しい構造に移行

START TRANSACTION;

-- 既存データの移行
INSERT INTO new_table (id, name, created_at)
SELECT
    old_id AS id,
    old_name AS name,
    NOW() AS created_at
FROM old_table
WHERE status = 'active';

-- データの検証
SELECT
    COUNT(*) AS migrated_count,
    COUNT(DISTINCT id) AS unique_count
FROM new_table;

-- 想定通りであればコミット、そうでなければロールバック
COMMIT;
-- ROLLBACK;
```

---

## 5. ロールバックスクリプト（Backward）

### 5.1 DDL削除

```sql
-- ============================================
-- Rollback for Migration #001
-- ============================================

START TRANSACTION;

-- 制約削除
ALTER TABLE existing_table
    DROP FOREIGN KEY fk_new_relation;

-- カラム削除
ALTER TABLE existing_table
    DROP COLUMN new_column;

-- インデックス削除
DROP INDEX idx_new_column ON existing_table;

-- テーブル削除
DROP TABLE IF EXISTS new_table;

COMMIT;
```

### 5.2 データ復元

```sql
-- データロールバックスクリプト
-- 必要に応じてバックアップからデータを復元

START TRANSACTION;

-- バックアップテーブルからデータを復元
INSERT INTO old_table (old_id, old_name)
SELECT id, name FROM new_table_backup;

COMMIT;
```

---

## 6. ゼロダウンタイムマイグレーション戦略

### 段階的マイグレーション手順

#### フェーズ1: カラム追加（NULL許可）
```sql
-- ダウンタイムなしで新カラムを追加（NULL許可）
ALTER TABLE users ADD COLUMN email_verified BOOLEAN NULL;
```

#### フェーズ2: アプリケーションデプロイ（Shadow Writing）
- 新カラムへの書き込みを開始（読み取りはまだ旧カラム）
- 両方のカラムにデータを書き込む（二重書き込み）

#### フェーズ3: データバックフィル
```sql
-- 既存レコードの新カラムに値を設定
UPDATE users
SET email_verified = FALSE
WHERE email_verified IS NULL;
```

#### フェーズ4: NOT NULL制約追加
```sql
-- 全データ移行完了後、NOT NULL制約を追加
ALTER TABLE users MODIFY COLUMN email_verified BOOLEAN NOT NULL DEFAULT FALSE;
```

#### フェーズ5: アプリケーション切り替え
- 新カラムからの読み取りに切り替え

#### フェーズ6: 旧カラム削除（オプション）
```sql
-- 安定稼働確認後、旧カラムを削除
ALTER TABLE users DROP COLUMN old_column;
```

---

## 7. 検証計画

### 7.1 マイグレーション前の検証
- [ ] DDLスクリプトの構文チェック
- [ ] 開発環境での実行確認
- [ ] ステージング環境での実行確認
- [ ] 実行時間の計測

### 7.2 マイグレーション後の検証
```sql
-- データ整合性チェック
SELECT COUNT(*) FROM new_table;
-- 想定: 10000件

-- インデックスの存在確認
SHOW INDEX FROM new_table;

-- 外部キー制約の確認
SELECT
    CONSTRAINT_NAME,
    TABLE_NAME,
    REFERENCED_TABLE_NAME
FROM information_schema.KEY_COLUMN_USAGE
WHERE TABLE_NAME = 'new_table';

-- パフォーマンステスト
EXPLAIN SELECT * FROM new_table WHERE new_column = 'test';
-- 想定: type=ref, key=idx_new_column
```

### 7.3 アプリケーションレベルの検証
- [ ] 主要機能の動作確認
- [ ] APIエンドポイントのテスト
- [ ] パフォーマンステスト（レスポンスタイム）
- [ ] エラーログの確認

---

## 8. 実施手順

### 事前準備
1. [ ] データベースバックアップ取得
   ```bash
   mysqldump -u root -p --single-transaction dbname > backup_YYYYMMDD.sql
   ```
2. [ ] メンテナンスモード有効化（ダウンタイムが必要な場合）
3. [ ] 関係者への通知

### マイグレーション実行
1. [ ] マイグレーションツール実行
   ```bash
   # Flyway
   flyway migrate

   # Liquibase
   liquibase update

   # Alembic (Python)
   alembic upgrade head
   ```
2. [ ] 実行ログの確認
3. [ ] データ検証スクリプト実行

### 事後作業
1. [ ] アプリケーション再起動
2. [ ] メンテナンスモード解除
3. [ ] 動作確認テスト実施
4. [ ] 監視ダッシュボード確認
5. [ ] 関係者への完了通知

---

## 9. ロールバック判断基準

以下のいずれかが発生した場合、ロールバックを実施：
- [ ] データ整合性エラーが検出された
- [ ] アプリケーションが起動しない
- [ ] 主要機能が動作しない
- [ ] パフォーマンスが大幅に劣化（XXms → YYms）
- [ ] 想定外のエラーログが大量発生

---

## 10. リスクと軽減策

| リスク | 発生確率 | 影響度 | 軽減策 |
|--------|---------|--------|--------|
| マイグレーション実行時間が予想より長い | 中 | 高 | 事前にステージング環境で実行時間を計測 |
| データ不整合が発生 | 低 | 高 | トランザクション内で実行、検証スクリプト準備 |
| アプリケーション互換性問題 | 中 | 高 | 段階的デプロイ、Feature Toggle |
| ロールバック失敗 | 低 | 致命的 | バックアップからの復元手順を事前確認 |

---

## 11. 承認

| 役割 | 氏名 | 承認日 | 署名 |
|------|------|--------|------|
| 開発リーダー | | | |
| DBエンジニア | | | |
| インフラ担当 | | | |
| プロジェクトマネージャー | | | |

---

## 12. 実施記録

- **実施日時**: [YYYY-MM-DD HH:MM:SS]
- **実施者**: [Name]
- **実行時間**: [XX分YY秒]
- **結果**: [成功 / 失敗 / ロールバック]
- **備考**: [特記事項]
