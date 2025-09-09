# ref配列操作

作成日 2025/09/09

## ref配列操作の三原則

1. ref配列の末尾追加には`push()`を使う
2. ref配列の削除・置換・途中追加には`splice()`を使う
3. ref配列のクリアには`splice(0)`を使う

```html
<script setup lang="ts">
    import { ref } from 'vue'
    const fruits = ref(['Apple', 'Banana', 'Cherry', 'Banana'])
    const addOrange = () => {
        fruits.value.push('Orange')
    }
    const removeBanana = () => {
        const bananaIndex = fruits.value.indexOf('Banana')
        if (bananaIndex !== -1) {
            fruits.value.splice(bananaIndex, 1)
        } else {
            alert('No more Bananas to remove!')
        }
    }
    const clearFruits = () => {
        // fruits.value = []
        fruits.value.splice(0)
    }
</script>
<template>
    <ul>
        <li v-for="(fruit, index) in fruits" :key="index">{{ fruit }}</li>
    </ul>
    <button @click="addOrange">Add Orange</button>
    <button @click="removeBanana">Remove Banana</button>
    <button @click="clearFruits">Clear Fruits</button>
</template>
```
