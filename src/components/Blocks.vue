<script setup>
// import { computed, ref, onBeforeUnmount, onMounted } from 'vue';

const props = defineProps({
    blocks: {
        type: Array,
        default: () => ([])
    }
})

// const opacity = ref(1)
// const interval = ref(null)

// function flash() {
//   interval.value = setInterval(() => {
//     opacity.value = opacity.value === 1 ? 0.6 : 1
//   }, 500)
// }

// const background = computed(() => `radial-gradient(circle, rgba(113, 81, 95, ${opacity.value}) 81%, rgba(0, 0, 0, ${opacity.value}) 100%)`)

// onMounted(() => {
//   flash()
// })

// onBeforeUnmount(() => {
//   clearInterval(interval.value)
// })
</script>

<template>
    <div class="block-wrapper">
        <div v-for="block in props.blocks" :key="block.id" :class="[
            'block-item',
            block.flash && 'block-item--flash',
            block.ball && 'block-item--ball'
        ]">
            <slot name="block" :block="block" />
        </div>
    </div>
</template>

<style lang="scss" scoped>
@keyframes flash {
    50% {
        background: radial-gradient(circle, rgba(113, 81, 95, 0.6) 81%, rgba(0, 0, 0, 0.6) 100%);
    }
}

.block-wrapper {
    margin: 0 auto;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: repeat(3, 1fr);
    grid-gap: 10px;
}

.block-item {
    width: 100px;
    height: 100px;
    border: black solid 2px;
    background: radial-gradient(circle, rgba(113, 81, 95, 1) 81%, rgba(0, 0, 0, 1) 100%);
    display: flex;
    place-items: center;

    &--flash {
        animation: flash 1s infinite;
    }

    // 其他方法
    // &--flash {
    //   background: v-bind(background);
    // }
}
</style>
