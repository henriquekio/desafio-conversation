<template>
    <transition name="fade-in">
        <div class="chat-msg" :class="containerClass">
            <div class="cm-msg-text">{{text}}</div>
        </div>
    </transition>
</template>

<script>
  export default {
    name: 'Message',
    props: ['type', 'text'],

    methods: {
      isUserType() {
        return this.type === 'user';
      },

      isBotType() {
        return this.type === 'bot';
      },
    },
    computed: {
      containerClass: function () { // eslint-disable-line
        return {
          sent: this.isUserType(),
          received: this.isBotType(),
        };
      },
      boxClass: function () { // eslint-disable-line
        return {
          user: this.isUserType(),
          bot: this.isBotType(),
        };
      },
      chatInfoClass: function () { // eslint-disable-line
        return {
          'user-info': this.isUserType(),
          'bot-info': this.isBotType(),
        };
      },
    },
  };
</script>

<style scoped>
    .fade-in-enter, .fade-in-leave-to {
        opacity: 0;
    }

    .fade-in-enter-active, .fade-in-leave-active {
        transition: .8s;
    }

    .chat-msg.user > .msg-avatar img {
        width: 45px;
        height: 45px;
        border-radius: 50%;
        float: left;
        width: 15%;
    }

    .chat-msg.bot > .msg-avatar img {
        width: 45px;
        height: 45px;
        border-radius: 50%;
        float: right;
        width: 15%;
    }

    .cm-msg-text {
        background: white;
        padding: 10px 15px 10px 15px;
        color: #666;
        max-width: 75%;
        float: left;
        margin-left: 10px;
        position: relative;
        margin-bottom: 20px;
        border-radius: 30px;
    }

    .chat-msg {
        clear: both;
    }

    .chat-msg.sent > .cm-msg-text {
        float: right;
        margin-right: 10px;
        background: #1976d2;
        color: white;
    }
</style>
