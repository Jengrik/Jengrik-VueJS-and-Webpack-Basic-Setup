<template>
    <h1 class="title"> {{ title }} </h1>
</template>

<script>
    export default {
        data() {
            return {
                title: 'Hello World!',
            }
        }
    }
</script>

<style scoped>
    .title {
        color: darkslateblue;
    }
</style>