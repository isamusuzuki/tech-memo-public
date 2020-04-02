# URL パスに基づいて、JSONを返す

作成日 2020/04/02

注目ポイント

- `jsonify`は、JSON文字列がASCII化されてしまうので、使わない
- CORS対策もしてあるので、クライアントサイドのみでも利用可能

```python
import json
import re

from flask import Response


def something(request):
    search = r'^\/(.+)$'
    res1 = re.match(search, request.path)
    if not res1:
        result = {
            'success': False,
            'message': '情報がありません'
        }
    else:
        result = {
            'success': True,
            'message': res1.group(1)
        }

    response = Response(
        json.dumps(result, ensure_ascii=False, indent=4),
        mimetype='application/json')
    response.headers.set('Access-Control-Allow-Origin', '*')
    response.headers.set('Access-Control-Allow-Headers',
                         'Origin, X-Requested-With, Content-Type, Accept')
    response.headers.set('Access-Control-Allow-Methods', 'GET')
    return response
```
