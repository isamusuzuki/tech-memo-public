# Bulma をつかいこなす

作成日 2020/01/29

## Level

要素を横に並べる

[https://bulma.io/documentation/layout/level/](https://bulma.io/documentation/layout/level/)

```html
<nav class="level">
    <div class="level-left">
        <div class="level-item">
            <img src="/static/images/toro-96x96.png" width="48px" heigh="48px" alt="toro" />
        </div>
        <div class="level-item">
            <div class="field">
                <div class="control">
                    <input class="input" type="text" placeholder="検索キーワード" v-model="keyword"
                        v-on:keyup.enter="search" style="width:400px;" />
                </div>
            </div>
        </div>
    </div>
</nav>
```
