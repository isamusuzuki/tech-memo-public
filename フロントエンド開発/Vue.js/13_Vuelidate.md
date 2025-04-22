# Vuelidate

作成日 2025/04/22

Vue.js 3 向けの、モデルベースのバリデーションツール

ホームページ => [Getting started | Vuelidate](https://vuelidate-next.netlify.app/)

インストール => `npm install @vuelidate/core @vuelidate/validators`

## 試してみたコード

```html
<script setup lang="ts">
import { reactive } from 'vue'
import { useVuelidate } from '@vuelidate/core'
import { required, email, minLength, maxLength } from '@vuelidate/validators'

interface iState {
  firstName: string
  lastName: string
  contact: {
    email: string
    phone: string
  }
}

const state = reactive<iState>({
  firstName: '',
  lastName: '',
  contact: {
    email: '',
    phone: ''
  }
})

const rules = {
  firstName: { required, minLength: minLength(2), maxLength: maxLength(20) },
  lastName: { required, minLength: minLength(2), maxLength: maxLength(20) },
  contact: {
    email: { required, email, minLength: minLength(5), maxLength: maxLength(50) },
    phone: { required, minLength: minLength(10), maxLength: maxLength(15) }
  }
}

const v$ = useVuelidate(rules, state)

function submit() {
  v$.value.$touch()
  if (v$.value.$invalid) {
    console.log('Form is invalid')
    return
  }
  console.log('Form submitted', state)
}
</script>

<template>
    <form @submit.prevent="submit">
    <div>
        <label for="firstName">First Name</label>
        <input type="text" id="firstName" v-model="state.firstName" />
        <span v-if="v$.firstName.$error">First name is required and must be between 2 and 20 characters.</span>
    </div>
    <div>
        <label for="lastName">Last Name</label>
        <input type="text" id="lastName" v-model="state.lastName" />
        <span v-if="v$.lastName.$error">Last name is required and must be between 2 and 20 characters.</span>
    </div>
    <div>
        <label for="email">Email</label>
        <input type="email" id="email" v-model="state.contact.email" />
        <span v-if="v$.contact.email.$error">Email is required and must be a valid email address.</span>
    </div>
    <div>
        <label for="phone">Phone</label>
        <input type="tel" id="phone" v-model="state.contact.phone" />
        <span v-if="v$.contact.phone.$error">Phone is required and must be between 10 and 15 characters.</span>
    </div>
    <button type="submit">Submit</button>
    </form>
</template>
```
