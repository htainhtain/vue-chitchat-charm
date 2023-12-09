<template>
  <div class="chat-card">
    <main class="chat-description" v-if="chatHistory.length === 0">
      <h1 class="chat-card-title">ChitChat Charm</h1>
      <p class="chat-card-description">Your lively chat companion!</p>
    </main>
    <ChatDisplay v-else :chatHistory="chatHistory" :isLoading="isLoading" />
    <ChatInput @submit-message="fetchData" />
  </div>
</template>

<script>
import ChatInput from './ChatInput.vue';
import ChatDisplay from './ChatDisplay.vue';
import axios from 'axios'

export default {
  name: 'chat',
  components: {
    ChatInput,
    ChatDisplay,
  },
  data() {
    return {
      newQuestion: "",
      chatHistory: [],
      isLoading: false,
      nextId: 0,
    };
  },
  methods: {
    fetchData(question) {
      this.isLoading = true
      const questionId = this.nextId++;
      this.chatHistory.push({ id: questionId, type: 'question', content: question });
      axios
        .post(`http://localhost:8080/ask`, { question: question })
        .then(response => {
          console.log(response.data.answer)
          this.chatHistory.push({ id: this.nextId++, type: 'answer', content: response.data.answer });
          this.isLoading = false
        })
        .catch(error => {
          console.log(error)
          this.errored = true
          this.isLoading = false
        })
    }
  }
}

</script>

<style scoped>
.chat-card {
  position: relative;
  padding: 2em 0em;
  border-radius: 30px;
  height: 80vh;
  background-color: #DED0B6;
  box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px, rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px, rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px;
  display: flex;
  align-items: center;
  flex-direction: column;
}

.chat-description {
  width: 50%;
  height: 90%;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.chat-card-title {
  font-size: 3em;
}

.chat-card-title,
.chat-card-description {
  text-align: center;
}

.chat-card-description {
  margin-top: 1em;
  font-size: 0.9em;
  color: rgb(67, 67, 67);
  letter-spacing: 1px;
  line-height: 1.6;
}
</style>
