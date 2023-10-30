<script setup lang="ts">
import { computed, onMounted, ref, defineProps } from 'vue';
import { getMessages, postMessage } from '../api';
import { type Message } from '../api';

const { loggedInUser } = defineProps<{
  loggedInUser: string
}>()

const messages = ref<Message[]>([])

const message = ref("")

const sendMessage = async () => {
  const response = await postMessage(message.value)
  if (!response.success)
    return alert(`Could not post message (${response.status})`)

  messages.value = [ ...messages.value, response.data ]
  message.value = ""
}

const presentedMessages = computed(() => [ ...messages.value ].reverse())

onMounted(async () => {
  const response = await getMessages()
  if (!response.success)
    return alert(`Could not load messages (${response.status})`)

  messages.value = response.data
})
</script>

<template>
  <section class="max-w-[800px] m-auto">
    <textarea v-model="message" class="textarea m-auto block mt-10 bg-base-300 w-[60%]" placeholder="Say something" rows="3"></textarea>
    <button @click="sendMessage" class="btn btn-info m-auto w-[60%] block mt-2 mb-6">Send</button>
    <div v-for="message in presentedMessages" class="alert my-4" :class="[loggedInUser === message.email ? 'alert-info' : '', 'alert my-4']">
      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" class="stroke-info shrink-0 w-6 h-6"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
      <span>{{ message.content }} ({{ message.email }})</span>
    </div>
  </section>
</template>

