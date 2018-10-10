<template>
    <div class="chat-circle" @click="openChat">
        <i class="material-icons">message</i>
    </div>
</template>

<script>

  export default {
    name: 'Button',
    methods: {
      openChat() {
        this.$emit('changeComponent', { componentName: 'Chat' });
      },
    },
  };
</script>

<style scoped>
    .chat-circle {
        position: fixed;
        bottom: 50px;
        right: 50px;
        background: #1976d2;
        width: 20px;
        height: 20px;
        border-radius: 50%;
        color: white;
        padding: 28px;
        cursor: pointer;
        box-shadow: 0px 3px 16px 0px rgba(0, 0, 0, 0.6), 0 3px 1px -2px rgba(0, 0, 0, 0.2), 0 1px 5px 0 rgba(0, 0, 0, 0.12);
    }
</style>
