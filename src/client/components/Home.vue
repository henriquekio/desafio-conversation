<template>
  <div class="container">
    <div class="wrapper">
      <div class="container-chat">
        <div class="history">
          <Message :type='message.type' :text="message.text" v-for="message in history" :key="message.timestamp"/>
          <div class="loading" v-show="isLoading"></div>
        </div>
        <div class="footer">
          <div class="text">
            <input v-model="currentMessage"/>
          </div>
          <div class="separator"></div>
          <div class="send-button" @click="talkToBot">
            <div>
              <span>Enviar</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from 'axios';
import Message from './Message.vue';

export default {
  name: 'Home',
  data() {
    return {
      logoDescription: 'Desafio Chatbot',
      history: [],
      currentMessage: '',
      isLoading: false,
    };
  },
  methods: {
    scrollToEnd() {
      const container = this.$el.querySelector('.history');
      setTimeout(() => {
        container.scrollTop = container.scrollHeight;
      }, 200);
    },

    handleSuccess(response) {
      const { message, context } = response.data;
      const newMessage = {
        type: 'bot',
        text: message,
        timestamp: (new Date()).getTime(),
      };

      this.isLoading = false;
      this.lastContext = context;
      this.history.push(newMessage);
      this.scrollToEnd();
    },

    handleFail(error) {
      this.isLoading = false;
      console.log('error', error);

      const errorMessage = {
        type: 'bot',
        text: 'Humm... Houve um erro, tente novamente.',
        timestamp: (new Date()).getTime(),
      };

      this.history.push(errorMessage);
      this.scrollToEnd();
    },

    talkToBot() {
      if (this.currentMessage) {
        const newMessage = {
          type: 'user',
          text: this.currentMessage,
          timestamp: (new Date()).getTime(),
        };

        this.history.push(newMessage);
        this.scrollToEnd();

        const parameters = { message: this.currentMessage };
        if (this.lastContext) { parameters.context = this.lastContext; }

        this.isLoading = true;
        this.currentMessage = '';

        axios.post('/api/v1/chatbot', parameters)
          .then(this.handleSuccess)
          .catch(this.handleFail);
      }
      console.log(this.currentMessage);
    },

  },
  components: {
    Message,
  },
};
</script>

<style scoped="sass">

</style>
