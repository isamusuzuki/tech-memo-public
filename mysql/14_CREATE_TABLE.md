# CREATE TABLE クエリー

作成日 2020/11/10

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
