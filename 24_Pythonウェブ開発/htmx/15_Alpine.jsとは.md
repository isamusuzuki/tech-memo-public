# Alpine.jsとは

作成日 2025/12/02

## 1. 公式サイト（英語）を読む

[Alpine.js](https://alpinejs.dev/)

Alpineは、モダンWebのjQueryみたいな、HTMLタグに直接振る舞いを与える、小さなツール

Alpineは、15アトリビュート、6プロパティ、2メソッドのコレクション

アトリビュートの例 x-data,x-text,x-model,x-if

## 2. htmxの書籍のサンプルコードでの使われ方

templates/rows.html

```html
<!--表の各行にチェックボックスがつく-->
<input type="checkbox" name="selected_contact_ids"
    value="{{ contact.id }}" x-model="selected">
```

templates/index.html

```html
<!--全体でselected変数を使う-->
<form x-data="{ selected: [] }">

    <!--全体で何個チェックがついているかを示す-->
    <template x-if="selected.length > 0">
        <slot x-text="selected.length"></slot>
        contacts selected

        <button type="button"
            @click="confirm(`Delete ${selected.length} contacts?`) &&
            htmx.ajax('DELETE', '/contacts', { source: $root, target: document.body })"
        >Delete</button>
    </template>

    <table>
    <tbody>
        {% include 'rows.html' %}
    </tbody>
    </table>
</form>
```
