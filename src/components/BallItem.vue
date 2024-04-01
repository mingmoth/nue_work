<template>
    <div class="ball-item">
        <slot />
    </div>
</template>

<style lang="scss" scoped>
.ball-item {
    width: 30px;
    height: 30px;
    background-color: #A5F12B;
    opacity: 1;
    border-radius: 50%;
    text-align: center;
    line-height: 30px;
    margin: 0 auto;
}
</style>