<script setup lang="ts">
import { ref } from 'vue';
import { signup } from '../api';

const email = ref("")
const password = ref("")

const handleSignup = async () => {
  const result = await signup(email.value, password.value)
  if (result.success)
    alert("Sikeres reg")
  else
    alert(`Sikertelen reg (${result.status})`)
}
</script>

<template>
  <section class="card w-[320px] m-auto mt-10 bg-base-300 text-primary p-6">
    <h1 class="card-title">Signup</h1>
    <div class="flex flex-col gap-4 py-4">
      <input class="input" placeholder="Email" type="text" v-model="email">
      <input class="input" placeholder="Password" type="password" v-model="password">
    </div>
    <div class="card-actions justify-end">
      <button class="btn btn-success" @click="handleSignup">Signup</button>
    </div>
  </section>
</template>

