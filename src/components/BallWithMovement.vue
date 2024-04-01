<script setup>
import { computed, ref } from 'vue';
import BallItem from './BallItem.vue';

const props = defineProps({
    startPosition: {
        type: Array,
        default: null
    },
    targetPosition: {
        type: Array,
        default: [0, 0]
    }
})

const ball = ref(null)

const isCustomMoveSetting = computed(() => props.startPosition?.length === 2 && props.targetPosition?.length === 2)
const formatPosition = computed(() => {
    if(isCustomMoveSetting.value) {
        return {
            x: props.targetPosition[0] - props.startPosition[0],
            y: props.targetPosition[1] - props.startPosition[1]
        }
    }
    const { bottom, left, right, top } = ball.value.$el.getBoundingClientRect()

    const clientX = (left + right) / 2
    const clientY = (top + bottom) / 2
    return {
        x: props.targetPosition[0] - clientX,
        y: props.targetPosition[1] - clientY
    }
})
</script>

<template>
    <BallItem ref="ball" class="ball--move">
        <slot />
    </BallItem>
</template>

<style lang="scss" scoped>

@keyframes movement {
    // 原需求規格
    100% {
        transform: translateX(200px)
    }
    /*
    100% {
        transform: translate3d(v-bind('`${ formatPosition.x }px`'), v-bind('`${ formatPosition.y }px`'), 0);
    }*/
}

.ball--move {
    position: v-bind('props.startPosition ? "absolute" : null');
    top: v-bind('props.startPosition ? props.startPosition[1] + "px" : null');
    left: v-bind('props.startPosition ? props.startPosition[0] + "px" : null');
    animation: movement 1s ease infinite;
    -webkit-animation: movement 1s ease infinite;
}
</style>