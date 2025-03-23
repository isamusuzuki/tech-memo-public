# databaseInfoMap プロパティ

作成日 2025/02/18、更新日 2025/02/19

[DBFlute プロパティ](https://dbflute.seasar.org/ja/manual/reference/dfprop/index.html) > [databaseInfoMap](https://dbflute.seasar.org/ja/manual/reference/dfprop/databaseinfo/index.html)

## 1. 自動生成する対象のテーブルを指定する

- tableExceptList ... 自動生成対象から除外するテーブルのリスト
- tableTargetList ... 自動生成対象を指定するテーブルのリスト

tableTargetList の指定がされている場合、tableExceptList は無効

```text
map:{
    ; variousMap = map:{
        ; tableTargetList = list:{FOO_TABLE ; prefix:FOO_ ; suffix:_FOO ; contain:_FOO_}
    }
}
```

## 2. DBFlute を複数のスキーマに対応させる

- additionalSchemaMap ... 自動生成対象に含める別スキーマの情報を設定

```text
variousMap = map:{
    additionalSchemaMap = map:{
        ; [schema-name] = map:{
            ; objectTypeTargetList = list:{[objectTypeTargetList]}
            ; tableExceptList = list:{[tableExceptList]}
            ; tableTargetList = list:{[tableTargetList]}
            ; columnExceptMap = map:{[columnExceptMap]}
            ; isSuppressCommonColumn = [true or false(default)]
            ; isSuppressProcedure = [true or false(default)]
        }
    }
}
```
