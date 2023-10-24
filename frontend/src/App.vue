<script setup lang="ts">
import Signup from "./components/Signup.vue"
import Login from "./components/Login.vue"
import Chat from "./components/Chat.vue"
import { ref } from "vue";

const page = ref("signup")
const isLoggedIn = ref(false)

const feLogin = () => {
  isLoggedIn.value = true
  page.value = "chat"
}
</script>

<template>
  <div class="navbar bg-base-300">
    <div class="flex-none">
      <button class="btn btn-square btn-ghost">
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" class="inline-block w-5 h-5 stroke-current"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16"></path></svg>
      </button>
    </div>
    <div class="flex-1">
      <a class="btn btn-ghost normal-case text-xl">My app</a>
    </div>
    <div class="flex gap-4">
      <button class="btn" @click="() => page = 'signup'">
        Signup
      </button>
      <button class="btn" @click="() => page = 'login'">
        Login
      </button>
      <button v-if="isLoggedIn" class="btn" @click="() => page = 'chat'">
        Chat
      </button>
    </div>
  </div>

  <main>
    <Signup v-if="page === 'signup'" />
    <Login v-if="page === 'login'" :feLogin="feLogin" />
    <Chat v-if="page === 'chat' && isLoggedIn" />
  </main>
</template>

