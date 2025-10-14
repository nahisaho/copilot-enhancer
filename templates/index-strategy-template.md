# インデックス戦略設計書

## プロジェクト情報
- **プロジェクト名**: [Project Name]
- **データベース**: [PostgreSQL 15 / MySQL 8.0 / SQL Server 2022]
- **作成日**: [YYYY-MM-DD]
- **作成者**: [Name]

---

## 1. インデックス戦略概要

### 1.1 設計方針
- クエリパフォーマンスとストレージコストのバランス
- 読み取りパフォーマンス優先（インデックス多め）
- 書き込みパフォーマンス優先（インデックス最小限）
- トランザクション頻度とクエリパターンに基づく最適化

### 1.2 パフォーマンス目標
- SELECT クエリ: p95 < 50ms
- INSERT/UPDATE: p95 < 100ms
- バッチ処理: 100万件/分以上

---

## 2. インデックス種類と選択基準

### 2.1 B-Tree インデックス（デフォルト）
**適用場面:**
- 範囲検索（`>, <, BETWEEN`）
- ソート処理（`ORDER BY`）
- 等値検索（`=`）
- LIKE検索（前方一致のみ）

**例:**
```sql
CREATE INDEX idx_created_at ON orders(created_at);
-- WHERE created_at BETWEEN '2024-01-01' AND '2024-12-31'
```

### 2.2 Hash インデックス
**適用場面:**
- 等値検索のみ（`=`）
- 範囲検索不要
- PostgreSQL 10以降で推奨

**例:**
```sql
CREATE INDEX idx_hash_user_id ON sessions USING HASH(user_id);
-- WHERE user_id = 12345
```

### 2.3 Full-Text インデックス
**適用場面:**
- 全文検索
- テキストマッチング

**例:**
```sql
-- PostgreSQL
CREATE INDEX idx_fulltext_content ON articles USING GIN(to_tsvector('english', content));

-- MySQL
CREATE FULLTEXT INDEX idx_fulltext_content ON articles(content);
```

### 2.4 Partial インデックス（部分インデックス）
**適用場面:**
- 特定条件のレコードのみインデックス化
- ストレージ削減・更新コスト削減

**例:**
```sql
CREATE INDEX idx_active_users ON users(email) WHERE status = 'active';
-- WHERE status = 'active' AND email = 'user@example.com'
```

### 2.5 Covering インデックス（カバリングインデックス）
**適用場面:**
- Index-Only Scan実現
- テーブルアクセス不要

**例:**
```sql
-- PostgreSQL
CREATE INDEX idx_covering ON orders(customer_id) INCLUDE (order_date, total_amount);

-- MySQL
CREATE INDEX idx_covering ON orders(customer_id, order_date, total_amount);
```

### 2.6 複合インデックス（Multi-Column Index）
**適用場面:**
- 複数カラムでの検索
- 左端一致の原則に従う

**例:**
```sql
CREATE INDEX idx_composite ON orders(customer_id, order_date, status);
-- 利用可能: WHERE customer_id = ? AND order_date = ?
-- 利用不可: WHERE order_date = ? (左端の customer_id がない)
```

---

## 3. テーブル別インデックス定義

### 3.1 テーブル: `users`

#### クエリパターン分析
| クエリパターン | 頻度 | 平均実行時間 | 優先度 |
|--------------|------|-------------|--------|
| SELECT WHERE email = ? | 高（1000回/分） | 5ms | 高 |
| SELECT WHERE status = 'active' ORDER BY created_at | 中（100回/分） | 120ms | 高 |
| SELECT WHERE last_login_at > ? | 低（10回/分） | 80ms | 中 |

#### インデックス定義

```sql
-- 主キー（自動作成）
PRIMARY KEY (id)

-- ユニークインデックス（メール検索）
CREATE UNIQUE INDEX uk_email ON users(email);

-- 複合インデックス（アクティブユーザー一覧）
CREATE INDEX idx_status_created ON users(status, created_at DESC)
WHERE status = 'active';

-- 部分インデックス（最終ログイン日時）
CREATE INDEX idx_last_login ON users(last_login_at)
WHERE last_login_at IS NOT NULL;
```

#### インデックス選択根拠
- `uk_email`: 等値検索頻度が高く、ユニーク制約も必要
- `idx_status_created`: status='active'の検索とソートを同時最適化
- `idx_last_login`: NULL値が多いため部分インデックスでストレージ削減

---

### 3.2 テーブル: `orders`

#### クエリパターン分析
| クエリパターン | 頻度 | 平均実行時間 | 優先度 |
|--------------|------|-------------|--------|
| SELECT WHERE customer_id = ? ORDER BY order_date DESC | 高 | 80ms | 高 |
| SELECT WHERE order_date BETWEEN ? AND ? | 中 | 150ms | 中 |
| SELECT WHERE status = 'pending' | 高 | 200ms | 高 |
| JOIN orders ON customers | 高 | 50ms | 高 |

#### インデックス定義

```sql
-- 主キー
PRIMARY KEY (id)

-- 外部キー（自動作成推奨）
CREATE INDEX idx_customer_id ON orders(customer_id);

-- 複合インデックス（顧客別注文履歴）
CREATE INDEX idx_customer_order_date ON orders(customer_id, order_date DESC);

-- カバリングインデックス（注文一覧API用）
CREATE INDEX idx_covering_orders ON orders(customer_id, order_date DESC)
INCLUDE (total_amount, status);

-- ステータス検索
CREATE INDEX idx_status ON orders(status)
WHERE status IN ('pending', 'processing');

-- 日付範囲検索
CREATE INDEX idx_order_date ON orders(order_date);
```

#### インデックス選択根拠
- `idx_customer_order_date`: 顧客別注文履歴の検索とソートを最適化
- `idx_covering_orders`: テーブルアクセス不要でIndex-Only Scan実現
- `idx_status`: 処理待ちの注文を頻繁に検索するため部分インデックス

---

## 4. 複合インデックスの列順序設計

### 4.1 左端一致の原則

複合インデックス `(A, B, C)` は以下のクエリで利用可能：
- WHERE A = ?
- WHERE A = ? AND B = ?
- WHERE A = ? AND B = ? AND C = ?

以下のクエリでは利用不可：
- WHERE B = ?
- WHERE C = ?
- WHERE B = ? AND C = ?

### 4.2 列順序の優先度

1. **等値検索（=）のカラムを先に**
   ```sql
   CREATE INDEX idx_example ON table(status, created_at);
   -- WHERE status = 'active' AND created_at > '2024-01-01'
   ```

2. **カーディナリティの高いカラムを先に**
   - カーディナリティ高: ユーザーID、メールアドレス
   - カーディナリティ低: ステータス（active/inactive）、性別

3. **範囲検索（>, <, BETWEEN）のカラムを後に**
   ```sql
   CREATE INDEX idx_example ON table(category_id, price);
   -- WHERE category_id = 1 AND price BETWEEN 1000 AND 5000
   ```

---

## 5. インデックスメンテナンス計画

### 5.1 統計情報更新

```sql
-- PostgreSQL
ANALYZE users;

-- MySQL
ANALYZE TABLE users;

-- 自動更新設定（PostgreSQL）
ALTER TABLE users SET (autovacuum_analyze_scale_factor = 0.05);
```

### 5.2 インデックス再構築

```sql
-- PostgreSQL（オンライン再構築）
REINDEX INDEX CONCURRENTLY idx_email;

-- MySQL
ALTER TABLE users DROP INDEX idx_email, ADD INDEX idx_email(email);
```

### 5.3 未使用インデックスの検出

```sql
-- PostgreSQL
SELECT
    schemaname,
    tablename,
    indexname,
    idx_scan,
    idx_tup_read,
    idx_tup_fetch
FROM pg_stat_user_indexes
WHERE idx_scan = 0
ORDER BY schemaname, tablename;

-- MySQL
SELECT
    DATABASE_NAME,
    TABLE_NAME,
    INDEX_NAME
FROM information_schema.STATISTICS
WHERE TABLE_SCHEMA = 'your_database'
AND INDEX_NAME NOT IN (
    SELECT DISTINCT INDEX_NAME
    FROM performance_schema.table_io_waits_summary_by_index_usage
);
```

---

## 6. パフォーマンス検証

### 6.1 実行計画確認

```sql
-- PostgreSQL
EXPLAIN (ANALYZE, BUFFERS)
SELECT * FROM users WHERE email = 'user@example.com';

-- MySQL
EXPLAIN FORMAT=JSON
SELECT * FROM users WHERE email = 'user@example.com';
```

### 6.2 インデックス使用状況

```sql
-- PostgreSQL
SELECT
    schemaname,
    tablename,
    indexname,
    idx_scan AS index_scans,
    idx_tup_read AS tuples_read,
    idx_tup_fetch AS tuples_fetched
FROM pg_stat_user_indexes
ORDER BY idx_scan DESC;
```

### 6.3 スロークエリ分析

```sql
-- PostgreSQL: pg_stat_statements拡張
SELECT
    query,
    calls,
    mean_exec_time,
    max_exec_time,
    stddev_exec_time
FROM pg_stat_statements
WHERE mean_exec_time > 100  -- 100ms以上
ORDER BY mean_exec_time DESC
LIMIT 20;
```

---

## 7. アンチパターンと注意点

### 7.1 避けるべきパターン

#### ❌ インデックスの乱立
- すべてのカラムにインデックスを作成
- 書き込みパフォーマンス劣化、ストレージ浪費

#### ❌ 関数適用後のインデックス
```sql
-- インデックスが使われない
WHERE LOWER(email) = 'user@example.com'

-- 正しい方法: 関数インデックス
CREATE INDEX idx_lower_email ON users(LOWER(email));
```

#### ❌ LIKE検索の後方一致・中間一致
```sql
-- インデックスが使われない
WHERE name LIKE '%Smith'
WHERE name LIKE '%John%'

-- 前方一致のみインデックス利用可能
WHERE name LIKE 'John%'
```

### 7.2 推奨事項

#### ✅ 複合インデックスの適切な列順序
```sql
-- 良い例
CREATE INDEX idx_status_date ON orders(status, created_at);
WHERE status = 'pending' AND created_at > '2024-01-01'

-- 悪い例（列順序が逆）
CREATE INDEX idx_date_status ON orders(created_at, status);
```

#### ✅ カバリングインデックスの活用
```sql
CREATE INDEX idx_covering ON orders(customer_id)
INCLUDE (order_date, total_amount);
-- Index-Only Scanでテーブルアクセス不要
```

---

## 8. 監視とアラート

### 監視項目
- [ ] インデックスサイズ増加率
- [ ] 未使用インデックスの検出（月次）
- [ ] スロークエリの検出（100ms以上）
- [ ] インデックススキャン vs フルテーブルスキャンの比率

### アラート設定
- スロークエリが閾値を超えた場合（p95 > 200ms）
- 未使用インデックスが検出された場合
- インデックスサイズが総テーブルサイズの50%を超えた場合

---

## 9. チェックリスト

- [ ] 主要クエリパターンを分析済み
- [ ] 複合インデックスの列順序が最適
- [ ] カバリングインデックスを検討済み
- [ ] 部分インデックスで不要なデータを除外
- [ ] 実行計画（EXPLAIN）で検証済み
- [ ] 統計情報更新スケジュール設定済み
- [ ] 未使用インデックス検出の監視設定済み
- [ ] インデックス再構築計画を策定済み
