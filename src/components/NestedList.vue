<script setup>
import { provide, reactive, ref } from 'vue';
import ListItem from './ListItem.vue';

const props = defineProps({
    accordion: {
        type: Boolean,
        default: false
    },
    activeKey: {
        type: String,
        default: null
    },
    activeKeys: {
        type: Object,
        default: () => ({})
    },
    items: {
        type: Array,
        required: true
    },
})

const activeKeys = reactive(structuredClone(props.activeKeys))
const activeKey = ref(structuredClone(props.activeKey));
const updateActiveItem = (key) => {
    activeKey.value = key
}

provide('activeKeys', activeKeys)
provide('setActiveKeys', (key, value) => {
    activeKeys[key] = value
})
provide('deleteActiveKeys', (key) => {
    delete activeKeys[key]
})

defineExpose({ activeKeys })
</script>

<template>
    <div class="nested-list">
        <ListItem v-for="item in props.items" :key="item.key" :item="item" :accordion="props.accordion"
            :active-key="activeKey" :depth="0" @active-item="updateActiveItem" />
    </div>
</template>