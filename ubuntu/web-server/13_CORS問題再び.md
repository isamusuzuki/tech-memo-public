# CORS 問題再び

作成日 2021/10/09

## Chrome ブラウザの開発ツールに、警告文が表示されている

```text
[Deprecation] "Authorization" will not be covered by the wildcard symbol (*)in CORS "Access-Control-Allow-Headers" handling.
```

## グーグルの説明を読む

[CORS non\-wildcard request\-header \- Chrome Platform Status](https://www.chromestatus.com/feature/5742041264816128)

> A CORS non-wildcard request header[1] is an HTTP request header which is not covered by the wildcard symbol ("\*") in the access-control-allow-headers header. "authorization" is the only member of CORS non-wildcard request-header.
>
> Currently we treat the header as a usual header, which is problematic for security reasons. Implement it, and change the current behavior.

=> "Access-Control-Allow-Origin" 項目以外では、ワイルドカードは使わないほうがよいみたいだ

## 自分のコードを修正する

```python
# 修正前
def enable_cors_jsonify(result: dict) -> Response:
    """
    CORSにも対応させつつ、もらったデータをJSON化する
    """
    json_data = json.dumps(result, ensure_ascii=False, indent=2)
    response = Response(json_data, mimetype='application/json')
    response.headers.set('Access-Control-Allow-Origin', '*')
    response.headers.set('Access-Control-Allow-Headers', '*')
    response.headers.set('Access-Control-Allow-Methods', '*')
    response.headers.set('Access-Control-Allow-Credentials', 'true')
    return response

# 修正後
def enable_cors_jsonify(result: dict) -> Response:
    """
    CORSにも対応させつつ、もらったデータをJSON化する
    """
    json_data = json.dumps(result, ensure_ascii=False, indent=2)
    response = Response(json_data, mimetype='application/json')
    response.headers.set('Access-Control-Allow-Origin', '*')
    response.headers.set(
        'Access-Control-Allow-Headers',
        'Origin, Authorization, Accept, Content-Type')
    response.headers.set(
        'Access-Control-Allow-Methods',
        'GET, POST, PUT, DELETE, OPTIONS')
    response.headers.set('Access-Control-Allow-Credentials', 'true')
    return response
```
