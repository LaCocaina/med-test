<template>
    <div class="text">
        <slot></slot>
    </div>
</template>

<script>
    export default {
        name: "TextBlock"
    }
</script>

<style scoped>
    .text {
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 32px;
        font-weight: 600;
    }
</style>