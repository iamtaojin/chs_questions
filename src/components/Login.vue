<script setup>
import { ref } from 'vue'

</script>

<template>
    <fieldset>
        <legend> user </legend>
    </fieldset>
</template>

<style scoped>
input {
    outline-style: none;
    border: 1px var(--bg-1) solid;
    color: gray;
}

input:hover {
    border: 1px solid gold;
}

input:active {
    border: 1px solid black;
}</style>