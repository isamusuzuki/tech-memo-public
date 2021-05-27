# FileUpload の記述

作成日 2021/05/27

## FileUpload を記述する

[File Upload](https://swagger.io/docs/specification/describing-request-body/file-upload/)

### 自分で正解はこれかなと思うもの

```yaml
paths:
  /api/cacao/upload:
    post:
      requestBody:
        content:
          multipart/form-data:
            schema:
              uploadFile:
                type: object
                properties:
                  type: object
                  properties:
                    filename:
                      type: string
                      format: binary
```

## 実際のところ、FileUpload はどう実装されているか

### クライアントサイド

- 基本 `<input type="file">` でオーケー
- その他はすべて Bulma の脚色
- [File upload \| Bulma: Free, open source, and modern CSS framework based on Flexbox](https://bulma.io/documentation/form/file/)

```html
<div class="file has-name">
  <label class="file-label">
    <input
      class="file-input"
      type="file"
      name="file"
      v-on:change.prevent="fileSelected"
    />
    <span class="file-cta">
      <span class="file-icon">
        <i class="fas fa-upload"></i>
      </span>
      <span class="file-label"> ファイルを選択 </span>
    </span>
    <span class="file-name" style="width: 300px;" v-text="selectedFilename">
    </span>
  </label>
</div>
```

- `FormData()` コンストラクターを使って、FormData オブジェクトを生成する
- `FormData.append()` メソッドを使って、キーと値の組を追加する
- [FormData\(\) \- Web API \| MDN](https://developer.mozilla.org/ja/docs/Web/API/FormData/FormData)

```javascript
const app = new Vue({
    el: '#app',
    data: {
        uploadFile: null,
        selectedFilename: '',
    },
    methods: {
        fileSelected: function(e) {
            this.uploadFile = e.target.files[0];
            this.selectedFilename = this.uploadFile.name;
        },
        fileUpload: function() {
            const formData = new FormData();
            formData.append('uploadFile', this.uploadFile);
            const config = {
                headers: {'content-type': 'multipart/form-data'}
            };
            axios
                .post('/api/cacao/upload', formData, config)
                .then((response) -> {
                    // 成功 or 失敗
                })
        }
    }
})
```

### サーバーサイド

- Python Flask Blueprint で記述
- [Uploading Files — Flask Documentation \(1\.1\.x\)](https://flask.palletsprojects.com/en/1.1.x/patterns/fileuploads/)

```python
from os import path
from flask import Blueprint, jsonify

cacao = Blueprint('cacao', __name__, url_prefix='/api/cacao')

@cacao.route('upload', methods=['POST'])
def upload():
    if 'uploadFile' in request.files:
        fl = request.files['uploadFile']
        file_name = fl.filename
        if file_name != '':
            full_path = path.join('temp', file_name)
            fl.save(full_path)
            return jsonify({'success': True})
```
