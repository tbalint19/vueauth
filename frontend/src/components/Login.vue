<script setup lang="ts">
import { ref, defineProps } from 'vue';
import { login } from '../api';
const { feLogin } = defineProps<{
  feLogin: () => void
}>()

const email = ref("")
const password = ref("")

const handleLogin = async () => {
  const result = await login(email.value, password.value)
  if (result.success) {
    localStorage.setItem("sessionId", result.data.sessionId)
    feLogin()
  }
  else
    alert(`Sikertelen Login (${result.status})`)
}
</script>

<template>
  <section class="card w-[320px] m-auto mt-10 bg-base-300 text-primary p-6">
    <h1 class="card-title">Login</h1>
    <div class="flex flex-col gap-4 py-4">
      <input class="input" placeholder="Email" type="text" v-model="email">
      <input class="input" placeholder="Password" type="password" v-model="password">
    </div>
    <div class="card-actions justify-end">
      <button class="btn btn-success" @click="handleLogin">Login</button>
    </div>
  </section>
</template>

