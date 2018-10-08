<template>
  <div class="container-message" :class="containerClass">
    <div class="chat-info" :class="chatInfoClass" v-if="isBotType()"></div>
    <div class="box" :class="boxClass">
      {{text}}
    </div>
    <div class="chat-info" :class="chatInfoClass" v-if="isUserType()"></div>
  </div>
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
      containerClass: function() { // eslint-disable-line
        return {
          sent: this.isUserType(),
          received: this.isBotType(),
        };
      },
      boxClass: function() { // eslint-disable-line
        return {
          user: this.isUserType(),
          bot: this.isBotType(),
        };
      },
      chatInfoClass: function() { // eslint-disable-line
        return {
          'user-info': this.isUserType(),
          'bot-info': this.isBotType(),
        };
      },
    },
  };
</script>

<style scoped>


</style>
