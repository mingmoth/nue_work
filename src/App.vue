<script setup>
import BallWithMovement from './components/BallWithMovement.vue';
import Blocks from './components/Blocks.vue'
import Header from './components/Header.vue'

const blocks = [
    {
        id: '1',
        flash: false,
        ball: true
    },
    {
        id: '2',
        flash: false,
        ball: false
    },
    {
        id: '3',
        flash: true,
        ball: true
    },
    {
        id: '4',
        flash: false,
        ball: false
    },
    {
        id: '5',
        flash: true,
        ball: false
    },
    {
        id: '6',
        flash: false,
        ball: false
    },
    {
        id: '7',
        flash: false,
        ball: true
    },
    {
        id: '8',
        flash: false,
        ball: false
    },
    {
        id: '9',
        flash: true,
        ball: true
    },
]

const centerPosition = [window.innerWidth / 2, window.innerHeight / 2]

</script>

<template>
    <Header />
    <div class="app-body">
        <Blocks :blocks="blocks">
            <template #block="{ block }">
                <component :is="block.ball ? BallWithMovement : null" :target-position="centerPosition" :key="block.id">
                    0
                </component>
            </template>
        </Blocks>
    </div>
</template>

<style lang="scss" scoped>
.app-body {
    display: flex;
    place-items: center;
    background-color: lightgray;
}
</style>
