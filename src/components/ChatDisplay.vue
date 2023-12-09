<template>
    <div class="chat-display-container" ref="chatContainer">
        <div v-for="(chat, index) in chatHistory" :key="index">
            <div class="message-question" v-if="chat.type === 'question' && !chat.answer">
                <p>{{ chat.content }}</p>
            </div>
            <div class="message-answer" v-if="chat.type === 'answer'">
                <p>{{ chat.content }}</p>
            </div>
        </div>
        <div v-if="isLoading">
            <Loading />
        </div>
    </div>
</template>

<script>
import Loading from './Loading.vue';

export default {
    name: 'chat-display',
    components: {
        Loading,
    },
    props: {
        chatHistory: {
            type: Array,
            default: () => [],
        },
        isLoading: {
            type: Boolean,
            default: false,
        },
    },
    data() {
        return {
            inputValue: ''
        };
    },
    methods: {
        handleSubmit() {
            this.$emit('submit-message', this.inputValue);
            this.inputValue = ''; // Clear the input after submission if needed
        },
        scrollToBottom() {
            const chatContainer = this.$refs.chatContainer;
            chatContainer.scrollTop = chatContainer.scrollHeight;
        },
    },
    watch: {
        // Watch the chatHistory array and scroll to the bottom when it changes
        chatHistory: {
            handler() {
                this.$nextTick(this.scrollToBottom);
            },
            deep: true,
        },
    },
}
</script>


<style scoped>
.chat-display-container {
    width: 70%;
    height: 90%;
    margin: 0 auto;
    overflow-y: auto;
}

.message-question {
    display: inline-block;
    background-color: #BBAB8C;
    padding: 0.8em 1em;
    border-radius: 10px;
    margin-bottom: 1em;
}

.message-answer {
    margin-bottom: 1em;
}
</style>