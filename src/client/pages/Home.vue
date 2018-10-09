<template>
    <component v-bind:is="componentView"></component>
</template>

<script>
    /* eslint-disable indent,import/extensions,import/no-unresolved */

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
    };
</script>

<style scoped>

</style>
