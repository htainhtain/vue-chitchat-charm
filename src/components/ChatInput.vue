<template>
    <div class="chat-input">
        <input class="input-field" v-model="inputValue" type="text" placeholder="Ask me anything.... "
            @keyup.enter="handleSubmit">
        <img src="../assets/send.svg" alt="send Icon" class="send-icon" @click="handleSubmit" />
    </div>
</template>


<script>

export default {
    name: 'chat-input',
    data() {
        return {
            inputValue: ''
        };
    },
    methods: {
        handleSubmit() {
            if (this.inputValue.trim() !== '') {
                this.$emit('submit-message', this.inputValue);
                this.inputValue = ''; 
            }
        }
    }
}
</script>


<style scoped>
.chat-input {
    margin: 0em auto;
    width: 70%;
    padding: 5px 20px;
    background-color: #FFFFFF;
    border-radius: 20px;
    display: flex;
    box-shadow: rgba(149, 157, 165, 0.2) 0px 8px 24px;
}

.input-field {
    width: 100%;
    height: 50px;
    background: transparent;
    border: none;
    font-size: 1em;
}

.input-field:focus {
    outline: none;
}

.send-icon {
    cursor: pointer;
}

.send-icon:hover {
    transform: scale(1.3);
    transition: transform 0.3s ease;
}
</style>