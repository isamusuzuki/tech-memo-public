# CREATE TABLE 構文

作成日 2020/11/10、更新日 2022/09/12

## 01. 基本形

```sql
DROP TABLE IF EXISTS logs;

CREATE TABLE `logs` (
    `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
    `category` varchar(100) NOT NULL,
    `name` varchar(100) NOT NULL,
    `success` tinyint(1) NOT NULL COMMENT '1:true,0:false',
    `message` text DEFAULT NULL,
    `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
    PRIMARY KEY (`id`)
);

INSERT INTO
    logs (category, name, success, message)
VALUES
    ('tech-memo', 'admin', 1, 'これはテストです。無視してください');

SELECT
    id,
    category,
    name,
    success,
    message,
    created_at
FROM
    logs
ORDER BY
    id DESC;
```

## 02. デフォルト値にタイムスタンプを使う

current_date, current_time, current_timestamp の 3 つは、\
`()` 括弧なしで、変数っぽく使うことも可能

```sql
CREATE TABLE sample (
    id int,
    val varchar(16),
    created_at timestamp NOT NULL default current_timestamp,
    updated_at timestamp NOT NULL default current_timestamp ON UPDATE current_timestamp,
    PRIMARY KEY(id)
);

INSERT INTO
    sample (id, val)
VALUES
    (1, 'aaa');
-- 作成日時と更新日時の両方に同じ日付が入る

UPDATE
    sample
SET
    val = 'bbb'
WHERE
    id = 1;
-- 更新日時だけが変わる

UPDATE
    sample
SET
    val = 'bbb'
WHERE
    id = 1;
-- UPDATEしても値が変わっていない場合は、更新日時も変わらない
```

## 03. テーブル作成時に、外部キーも設定する

```sql
CREATE TABLE `m_suppliers` (
    `supplier_code` int NOT NULL COMMENT '仕入先コード',
    `supplier_name` varchar(100) NOT NULL COMMENT '仕入先名称',
    `supplier_status` ENUM('正常', '取引停止', '保留中') NOT NULL COMMENT '取引状況',
    `is_deleted` tinyint DEFAULT '0' COMMENT '削除フラグ(0:生き,1:削除)',
    `updated_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新日時',
    `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '作成日時',
    PRIMARY KEY (`supplier_code`)
);

CREATE TABLE `m_items` (
    `item_code` varchar(30) NOT NULL COMMENT '商品コード',
    `supplier_code` int NOT NULL COMMENT '仕入先コード',
    `shiire_type` varchar(30) NOT NULL COMMENT '仕入タイプ',
    `is_deleted` tinyint DEFAULT '0' COMMENT '削除フラグ(0:否,1:削除)',
    `updated_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新日時',
    `created_at` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '作成日時',
    PRIMARY KEY (`item_code`),
    CONSTRAINT `fk_m_items_m_suppliers` FOREIGN KEY (`supplier_code`) REFERENCES `m_suppliers` (`supplier_code`)
);
```
