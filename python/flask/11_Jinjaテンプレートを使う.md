# Jinja テンプレートを使う

作成日 2020/02/13

## 01. Jinja とは

公式トップ => [Jinja — Jinja Documentation \(2\.11\.x\)](https://palletsprojects.com/p/jinja/)

## 02. 文法を学ぶ

[Template Designer Documentation — Jinja Documentation \(2\.11\.x\)](https://jinja.palletsprojects.com/en/master/templates/#synopsis)

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

- `{% ... %}` = ステートメント
- `{{ ... }}` = エクスプレッション
- `{# ... #}` = コメント

ステートメントには、for, if, macro, call, filter, set がある
