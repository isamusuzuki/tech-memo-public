# JSON (JavaScript Object Notation)

作成日 2019/04/15

## 1. JSONとは

オブジェクトから関数を取り除いて文字列化したもの

```json
{
  "name": {
    "firstName": "Bob",
    "lastName": "Smith"
  },
  "age": 32,
  "gender": "male",
  "interests": ["music", "skiing"]
}
```

じつは、たったひとつの文字列や、何かの配列も、有効なJSONである

わからなくなったらオンラインのバリデータでチェックする => [JSON Online Validator and Formatter - JSON Lint](https://jsonlint.com/)

## 2. オブジェクトからJSONへの変換

```javascript
const string1 = JSON.stringify(object1);
//=> 改行なしの文字列にする
const string2 = JSON.stringify(object2, null, 2);
//=> 改行とインデントがある見やすい文字列にする
```

### 3. JSONからオブジェクトへの変換

```javascript
const object1 = JSON.parse(string1);
```
