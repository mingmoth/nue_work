<script setup>
</script>

<template>
    <div class="header">
        <button class="header-btn">Open</button>
    </div>
</template>

<style lang="scss" scoped>
.header {
    width: 100vw;
    display: flex;
    justify-content: end;
    padding: 4px;
    box-sizing: border-box;

    &-btn {
        border: 1px solid black;
        cursor: pointer;
    }
}
</style>