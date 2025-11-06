# Jinjaを使う

作成日 2020/02/13、更新日 2025/08/28

## 1. Jinjaとは

HTMLテンプレートエンジン

公式サイト => [Jinja — Jinja Documentation (3.1.x)](https://jinja.palletsprojects.com/)

インストール => `pip install Jinja2`

## 2. テンプレートの文法を学ぶ

[Template Designer Documentation — Jinja Documentation (3.1.x)](https://jinja.palletsprojects.com/en/stable/templates/)

テンプレートの例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>My Webpage</title>
</head>
<body>
    <ul id="navigation">
    {% for item in navigation %}
        <li><a href="{{ item.href }}">{{ item.caption }}</a></li>
    {% endfor %}
    </ul>

    <h1>My Webpage</h1>
    {{ a_variable }}

    {# a comment #}
</body>
</html>
```

- `{% xxx %}` ... ステートメント
- `{{ xxx }}` ... エクスプレッション
- `{# xxx #}` ... コメント

ステートメントには、for, if, macro, call, filter, set がある

## 3. サンプルコード

```python
from jinja2 import Environment, FileSystemLoader, select_autoescape

env = Environment(
    loader=FileSystemLoader('html_templates'),
    autoescape=select_autoescape(
        enabled_extensions=['html', 'xml'],
        disabled_extensions=['txt'])
)

template = env.get_template('template1.html')
html_txt = template.render({'name': 'Taro Okamoto'})
```

- FileSystemLoaderを使うと、テンプレートファイルの置き場を指定できる
- select_autoescapeを使うと、ファイルの拡張子でオートエスケープのオン・オフを設定できる

## 4. テンプレートを継承させる

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
