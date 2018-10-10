<template>
    <transition name="slide-fade">
        <component v-on:changeComponent="changeComponent" v-bind:is="componentView"></component>
    </transition>
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
        currentComponent: 'Button',
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
    .slide-fade-enter-active {
        transition: all .2s ease;
    }
    .slide-fade-leave-active {
        transition: all .2s cubic-bezier(1.0, 0.5, 0.8, 1.0);
    }
    .slide-fade-enter, .slide-fade-leave-to
        /* .slide-fade-leave-active below version 2.1.8 */ {
        transform: translatey(10px);
        opacity: 0;
    }
</style>
