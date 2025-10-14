# テーブル定義書

## テーブル名: [TABLE_NAME]

### 概要
[テーブルの目的と役割を記述]

### テーブル情報
- **物理名**: `[table_name]`
- **論理名**: [テーブル論理名]
- **推定レコード数**: [初期/1年後/3年後]
- **主なアクセスパターン**: [READ主体 / WRITE主体 / READ/WRITE混在]

---

## カラム定義

| # | 物理名 | 論理名 | データ型 | NULL | デフォルト値 | 制約 | 説明 |
|---|--------|--------|----------|------|------------|------|------|
| 1 | id | ID | BIGINT | NOT NULL | AUTO_INCREMENT | PK | 主キー |
| 2 | name | 名前 | VARCHAR(255) | NOT NULL | - | - | ユーザー名 |
| 3 | email | メール | VARCHAR(255) | NOT NULL | - | UK | メールアドレス（一意） |
| 4 | status | ステータス | VARCHAR(20) | NOT NULL | 'active' | CHECK | アカウント状態 |
| 5 | created_at | 作成日時 | TIMESTAMP | NOT NULL | CURRENT_TIMESTAMP | - | レコード作成日時 |
| 6 | updated_at | 更新日時 | TIMESTAMP | NOT NULL | CURRENT_TIMESTAMP | ON UPDATE | レコード更新日時 |

---

## 制約

### 主キー (Primary Key)
```sql
PRIMARY KEY (id)
```

### ユニークキー (Unique Key)
```sql
UNIQUE KEY uk_email (email)
```

### 外部キー (Foreign Key)
```sql
FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE ON UPDATE CASCADE
```

### CHECK制約
```sql
CHECK (status IN ('active', 'inactive', 'suspended'))
```

---

## インデックス

| インデックス名 | タイプ | 対象カラム | 目的 |
|--------------|--------|-----------|------|
| idx_email | B-TREE | email | メールアドレス検索高速化 |
| idx_created_at | B-TREE | created_at | 日付範囲検索 |
| idx_status_created | B-TREE | (status, created_at) | ステータス別一覧取得 |

---

## パーティション設計

### パーティション戦略
- **タイプ**: [RANGE / LIST / HASH]
- **キー**: [partition_key]
- **目的**: [データ分散・クエリ性能向上・古いデータのアーカイブ容易化]

### パーティション定義例
```sql
PARTITION BY RANGE (YEAR(created_at)) (
    PARTITION p2023 VALUES LESS THAN (2024),
    PARTITION p2024 VALUES LESS THAN (2025),
    PARTITION p2025 VALUES LESS THAN (2026),
    PARTITION p_future VALUES LESS THAN MAXVALUE
);
```

---

## DDL

```sql
CREATE TABLE [table_name] (
    id BIGINT AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL,
    status VARCHAR(20) NOT NULL DEFAULT 'active',
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,

    PRIMARY KEY (id),
    UNIQUE KEY uk_email (email),
    CHECK (status IN ('active', 'inactive', 'suspended'))
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;

-- インデックス作成
CREATE INDEX idx_created_at ON [table_name](created_at);
CREATE INDEX idx_status_created ON [table_name](status, created_at);
```

---

## パフォーマンス考慮事項

### 想定クエリパターン
1. **ユーザー検索（メールアドレス）**
   ```sql
   SELECT * FROM users WHERE email = ?;
   ```
   - インデックス: `uk_email` を使用

2. **ステータス別一覧取得**
   ```sql
   SELECT * FROM users WHERE status = 'active' ORDER BY created_at DESC LIMIT 100;
   ```
   - インデックス: `idx_status_created` を使用

### 最適化ポイント
- [ ] 頻繁に検索されるカラムにインデックス設定
- [ ] 複合インデックスの列順序を検索条件に合わせる
- [ ] カバリングインデックスでIndex-Only Scan実現
- [ ] パーティションプルーニングで検索範囲削減

---

## データ整合性戦略

### トランザクション境界
- [このテーブルを含む典型的なトランザクション範囲を記述]

### 分離レベル
- **推奨**: [READ COMMITTED / REPEATABLE READ]
- **理由**: [ファントムリード防止 / デッドロック最小化 等]

---

## セキュリティ

### アクセス制御
```sql
-- アプリケーションユーザーの権限
GRANT SELECT, INSERT, UPDATE ON [table_name] TO 'app_user'@'%';

-- 読み取り専用ユーザー
GRANT SELECT ON [table_name] TO 'readonly_user'@'%';
```

### 暗号化
- [ ] TDE（Transparent Data Encryption）有効化
- [ ] 機密カラム（email等）の列レベル暗号化検討

### 監査ログ
- [ ] 変更履歴トリガー設定
- [ ] 監査テーブルへのログ記録

---

## バックアップ・復旧

- **バックアップ頻度**: [毎日/毎週]
- **保存期間**: [30日間]
- **復旧目標時間（RTO）**: [4時間以内]
- **復旧目標時点（RPO）**: [1時間以内]

---

## 運用・監視

### 監視項目
- [ ] テーブルサイズ増加率
- [ ] インデックス使用率
- [ ] スロークエリ検出
- [ ] デッドロック発生頻度

### メンテナンス計画
- [ ] 古いデータのアーカイブ（月次）
- [ ] インデックス再構築（必要に応じて）
- [ ] 統計情報更新（週次）
