# Jinja テンプレートを使う

作成日 2020/02/13、更新日 2020/10/28

## 01. Jinja とは

公式トップ => [Jinja \| The Pallets Projects](https://palletsprojects.com/p/jinja/)

ドキュメントトップ => [API — Jinja Documentation \(2\.11\.x\)](https://jinja.palletsprojects.com/en/2.11.x/api/)

## 02. テンプレートの文法を学ぶ

テンプレートトップ => [Template Designer Documentation — Jinja Documentation \(2\.11\.x\)](https://jinja.palletsprojects.com/en/2.11.x/templates/)

Jinja テンプレートの例

```html
<ul id="navigation">
  {% for item in navigation %}
  <li>
    <a href="{{ item.href }}">
      {{ item.caption }}
    </a>
  </li>
  {% endfor %}
</ul>

{{ a_variable }} {# a comment #}
```

- `{% xxx %}` ... ステートメント
- `{{ xxx }}` ... エクスプレッション
- `{# xxx #}` ... コメント

ステートメントには、for, if, macro, call, filter, set がある

## 03. Jinja を単体で使う

```python
from jinja2 import Environment, FileSystemLoader, select_autoescape

env = Environment(
    loader=FileSystemLoader('html_templates'),
    autoescape=select_autoescape(
        enabled_extensions=['html', 'xml'],
        disabled_extensions=['txt'])
)

template = env.get_template('template1.html')
html_txt = teamplate.render({'name': 'Taro Okamoto'})
```

- FileSystemLoader を使うと、テンプレートファイルの置き場を指定できる
- select_autoescape と使うと、ファイルの拡張子でオートエスケープのオン・オフを設定できる

## 04. テンプレートを継承させる

- `{% block %}` ... 子テンプレートが上書きする箇所を指定する
- `{% extends %}` ... 親テンプレートを指定する
- `{ super() }` ... 親テンプレートの内容を表示する

[Template Designer Documentation — Jinja Documentation \(2\.11\.x\)](https://jinja.palletsprojects.com/en/2.11.x/templates/)

> Template Inheritance
>
> The most powerful part of Jinja is template inheritance. Template inheritance allows you to build a base “skeleton” template that contains all the common elements of your site and defines blocks that child templates can override.

base.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    {% block head %}
    <link rel="stylesheet" href="base.css" />
    {% endblock %}
</head>
<body>
    <div id="content">{% block content %}{% endblock %}</div>
</body>
</html>
```

index.html

```html
{% extends "base.html" %}
{% block head %}
  {{ super() }}
  link rel="stylesheet" href="index.css" />
{% endblock %}
{% block content %}
    <h1>Index</h1>
{% endblock %}
```
