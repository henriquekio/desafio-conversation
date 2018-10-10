<template>
    <div id="app">
        <main>
            <router-view></router-view>
        </main>
    </div>
</template>

<script>

    export default {
      name: 'app',
    };
</script>

<style>
    @import url('https://fonts.googleapis.com/css?family=Roboto');
    html, body {
        height: 100%;
        width: 100%;
        font-family: 'Roboto', sans-serif;
    }
</style>
