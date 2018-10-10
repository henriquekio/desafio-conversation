<template>
    <div class="chat-box">
        <div class="chat-header">
            JapaBot
            <span class="chat-close-icon" @click="closeChat"><i class="material-icons">close</i></span>
        </div>
        <div class="chat-body">
            <div class="chat-content history">
                <Message :type='message.type' :text="message.text" v-for="message in history" :key="message.timestamp"/>
                <div class="loading" v-show="isLoading"></div>
            </div><!--chat-log -->
        </div>
        <div class="input-container">
            <form autocomplete="off">
                <input v-model="currentMessage" type="text" id="chat-input" placeholder="Digite sua mensagem...">
                <button @click="talkToBot" class="chat-submit"><i class="material-icons">send</i></button>
            </form>
        </div>
    </div>
</template>


<script>
  import axios from 'axios';
  import Message from './Message.vue';

  export default {
    name: 'Chat',
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
          if (this.lastContext) {
            parameters.context = this.lastContext;
          }

          this.isLoading = true;
          this.currentMessage = '';

          axios.post('/api/v1/chatbot', parameters)
            .then(this.handleSuccess)
            .catch(this.handleFail);
        }
        console.log(this.currentMessage);
      },

      closeChat() {
        this.$emit('changeComponent', { componentName: 'Button' });
      },

    },
    components: {
      Message,
    },
  };
</script>

<style scoped>
    .loading {
        font-size: 30px;
        text-align: center;
    }

    .loading:after {
        overflow: hidden;
        display: inline-block;
        vertical-align: bottom;
        -webkit-animation: ellipsis steps(4, end) 2000ms infinite;
        animation: ellipsis steps(4, end) 2000ms infinite;
        content: "\2026"; /* ascii code for the ellipsis character */
        width: 0px;
    }

    .chat-box {
        background: #efefef;
        position: fixed;
        right: 30px;
        bottom: 50px;
        width: 350px;
        max-width: 85vw;
        max-height: 50vh;
        border-radius: 5px;
        box-shadow: 0px 5px 35px 9px #ccc;
    }

    .chat-close-icon {
        float: right;
        margin-right: 15px;
        cursor: pointer;
    }

    .chat-header {
        background: #1976d2;
        height: 35px;
        border-top-left-radius: 5px;
        border-top-right-radius: 5px;
        color: white;
        text-align: center;
        font-size: 1em;
        padding-top: 17px;
    }

    .chat-body {
        position: relative;
        height: 280px;
        border: 1px solid #ccc;
        overflow: hidden;
    }

    .chat-content {
        padding: 15px;
        height: 280px;
        overflow-y: scroll;
    }

    .chat-content::-webkit-scrollbar-track {
        -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
        background-color: #F5F5F5;
    }

    .chat-content::-webkit-scrollbar {
        width: 5px;
        background-color: #F5F5F5;
    }

    .chat-content::-webkit-scrollbar-thumb {
        background-color: #1976d2;
    }

    #chat-input {
        background: #f4f7f9;
        width: 81%;
        position: relative;
        height: 25px;
        padding-top: 10px;
        padding-right: 50px;
        padding-bottom: 10px;
        padding-left: 15px;
        border: none;
        resize: none;
        outline: none;
        border: 1px solid #ccc;
        color: #888;
        border-top: none;
        border-bottom-right-radius: 5px;
        border-bottom-left-radius: 5px;
        overflow: hidden;
    }

    .input-container > form {
        margin-bottom: 0;
    }

    #chat-input::-webkit-input-placeholder { /* Chrome/Opera/Safari */
        color: #ccc;
    }

    #chat-input::-moz-placeholder { /* Firefox 19+ */
        color: #ccc;
    }

    #chat-input:-ms-input-placeholder { /* IE 10+ */
        color: #ccc;
    }

    #chat-input:-moz-placeholder { /* Firefox 18- */
        color: #ccc;
    }

    .chat-submit {
        position: absolute;
        bottom: 3px;
        right: 10px;
        background: transparent;
        box-shadow: none;
        border: none;
        border-radius: 50%;
        color: #1976d2;
        width: 35px;
        height: 35px;
        outline: none;
        cursor: pointer;
    }
</style>
