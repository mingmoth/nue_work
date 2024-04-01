<script setup>
import { onMounted, ref, toRaw } from 'vue';
// components
import Dialog from './Dialog.vue';
import NestedList from './NestedList.vue';
// helper
import { setWithExpiry, getWithExpiry } from '../helpers/storage';

// function createNestedItem(level, maxLevel) {
//     if (level > maxLevel) {
//         return { key: level.toString(), text: `${level}-child`, children: [] };
//     }

//     const child = createNestedItem(level + 1, maxLevel);
//     return { key: level.toString(), text: `${level}-child`, children: [child] };
// }

// 調用函數以生成約十個階層的物件
// const maxLevel = 100;
// const generatedItem = createNestedItem(1, maxLevel);

const items = [
    {
        key: "64f",
        text: "好喝黑糖",
        children: [{
            key: "445",
            text: "黑糖鮮奶",
            children: [{
                key: "37a",
                text: "黑糖珍珠鮮奶"
            }, {
                key: "feb",
                text: "黑糖芋圓鮮奶"
            }, {
                key: "59c",
                text: "黑糖蒟蒻鮮奶"
            }]
        }, {
            key: "29e",
            text: "黑糖冬瓜",
            children: [{
                key: "ac3",
                text: "黑糖冬瓜牛奶"
            }, {
                key: "ca0",
                text: "黑糖冬瓜珍珠"
            }]
        }]
    },
    {
        key: "6c3",
        text: "茶",
        children: [{
            key: "5dc",
            text: "烏龍綠"
        }, {
            key: "b5f",
            text: "綠茶"
        }, {
            key: "b09",
            text: "紅茶"
        }, {
            key: "887",
            text: "青茶"
        }]
    },
    {
        key: "c81",
        text: "咖啡",
        children: [{
            key: "e02",
            text: "黑咖啡",
            children: [{
                key: "d20",
                text: "濃縮咖啡"
            }, {
                key: "1f8",
                text: "美式咖啡"
            }]
        }, {
            key: "d7a",
            text: "牛奶咖啡",
            children: [{
                key: "c09",
                text: "拿鐵",
                children: [{
                    key: "db2",
                    text: "黑糖拿鐵"
                }, {
                    key: "9f6",
                    text: "榛果拿鐵"
                }, {
                    key: "b61",
                    text: "香草拿鐵"
                }]
            }, {
                key: "9ac",
                text: "卡布奇諾"
            }, {
                key: "ce8",
                text: "摩卡"
            }]
        }]
    }
];

const sidebarOpen = ref(false)

const nestedListName = 'sun'
const storedActiveKeys = getWithExpiry(nestedListName) ?? {}
const firstActiveKey = storedActiveKeys[0] ?? null
const nestedList = ref(null)

function memorizeActiveKeys() {
    const activeKeys = toRaw(nestedList.value.activeKeys)
    setWithExpiry(nestedListName, toRaw(activeKeys))
}

onMounted(() => {
    window.addEventListener('beforeunload', memorizeActiveKeys)
})


</script>

<template>
    <div class="header">
        <button class="header-btn" @click="sidebarOpen = !sidebarOpen">Open</button>
    </div>
    <Dialog :class="[
            'sidebar',
            sidebarOpen && 'sidebar--open'
        ]" :open="sidebarOpen" @close="sidebarOpen = false">
        <!-- <button @click="sidebarOpen = false">Close</button> -->
        <NestedList accordion ref="nestedList" :active-keys="storedActiveKeys" :active-key="firstActiveKey"
            :items="items" />
    </Dialog>
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

.sidebar {
    height: 100vh;
    max-height: 100vh; // safari
    width: max-content;
    margin: 0 0 0 auto;
    padding: 16px 8px;
    background-color: darkgray;
    border: none;
    color: white;
    overflow: scroll;
    transform: translateX(100%);
    transition: transform 0.5s ease-in-out;

    &--open {
        transform: translateX(0%);
    }
}
</style>