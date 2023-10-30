<script setup lang="ts">
import Signup from "./components/Signup.vue"
import Login from "./components/Login.vue"
import Chat from "./components/Chat.vue"
import { onMounted, ref } from "vue";
import { jwtDecode } from "jwt-decode";

const page = ref("login")
const isLoggedIn = ref(false)

const loggedInUser = ref("")

const logout = () => {
  localStorage.removeItem("sessionId")
  feLogout()
}

const feLogout = () => {
  isLoggedIn.value = false
  page.value = "login"
  loggedInUser.value = ""
}

const feLogin = () => {
  isLoggedIn.value = true
  page.value = "chat"
  const existingToken = localStorage.getItem("sessionId")
  if (existingToken)
  loggedInUser.value = (jwtDecode(existingToken) as any)?.email
}

onMounted(() => {
  const existingToken = localStorage.getItem("sessionId")
  if (existingToken)
    feLogin()
})
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
      <button v-if="!isLoggedIn" class="btn" @click="() => page = 'signup'">
        Signup
      </button>
      <button v-if="!isLoggedIn" class="btn" @click="() => page = 'login'">
        Login
      </button>
      <button v-if="isLoggedIn" class="btn" @click="logout">
        Logout
      </button>
    </div>
  </div>

  <main>
    <Signup v-if="page === 'signup'" />
    <Login v-if="page === 'login'" :feLogin="feLogin" />
    <Chat v-if="page === 'chat' && isLoggedIn" :loggedInUser="loggedInUser" />
  </main>
</template>

