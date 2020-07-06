<template>
    <button @click="count">{{ buttonText }}</button>
</template>

<script>
export default {
    name: "Button",
    props: {
        buttonText: String,
    },

    data() {
        return {
            counter: 0,
        }
    },

    methods: {
        count() {
            this.counter++;
        }
    }
}
</script>

<style>
button {
    background-color: rgb(65, 184, 131);
    font-size: 30px;
    border-radius: 12px;
    border: none;
    padding: 10px 20px;
}

</style>