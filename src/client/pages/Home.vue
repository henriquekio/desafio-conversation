<template>
    <component v-on:changeComponent="changeComponent" v-bind:is="componentView"></component>
</template>

<script>
  /* eslint-disable import/extensions,import/no-unresolved */
  import Chat from '../components/Chat';
  import Button from '../components/Button';

  export default {
    name: 'Home',
    components: {
      Chat,
      Button,
    },
    data() {
      return {
        currentComponent: 'Chat',
      };
    },
    computed: {
      componentView() {
        return this.currentComponent;
      },
    },
    methods: {
      changeComponent(payload) {
        this.currentComponent = payload.componentName;
      },
    },
  };
</script>

<style scoped>

</style>
