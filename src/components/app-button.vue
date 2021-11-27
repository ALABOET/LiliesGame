<template>
    <button @click="$emit('action')">{{text}}</button>
</template>

<script>
    export default {
        name: "app-button",
        props:{
            text:String
        }
    }
</script>

<style scoped>
    button {
        width: 30%;
        padding: 1rem;
        text-decoration: none;
        position: relative;
        color: #FFF;
        font-size: 25px;
        font-weight: bold;
        text-transform: uppercase;
        font-family: 'Roboto Condensed', Тahoma, sans-serif;
        background: #337AB7;
        cursor: pointer;
        border: 2px solid #BFE2FF;
        overflow: hidden;
        z-index: 1;
    }
    button:hover,
    button:active,
    button:focus {
        color: #FFF;
    }
    button:before {
        content: '';
        position: absolute;
        top: 0;
        right: -50px;
        bottom: 0;
        left: 0;
        border-right: 50px solid transparent;
        border-top: 50px solid #2D6B9F;
        transition: transform 0.5s;
        transform: translateX(-100%);
        z-index: -1;
    }
    button:hover:before,
    button:active:before,
    button:focus:before {
        transform: translateX(0);
    }
    @media screen and (max-width: 500px) {
        button {
            width: 100%;
        }
    }
</style>