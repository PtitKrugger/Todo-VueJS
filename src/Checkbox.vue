<template>
    <label>
        <input type="checkbox" v-model="model" />
        {{label}}
    </label>
</template>

<script setup>
const model = defineModel()
defineProps({
    label: String
})
</script>