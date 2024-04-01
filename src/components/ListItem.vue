<script setup>
import { computed, inject, ref, watch, toRaw, watchEffect } from 'vue';

const props = defineProps({
    item: {
        type: Object,
        required: true,
    },
    accordion: {
        type: Boolean,
        default: false
    },
    activeKey: {
        type: String,
        default: null
    },
    depth: {
        type: Number,
        default: 0
    }
})

const emit = defineEmits(['active-item'])

const expanded = ref(false)
const toggleItem = () => {
    if (isExpandable.value) {
        expanded.value = !expanded.value
    }
    emit('active-item', props.item.key)
}

const isExpandable = computed(() => {
    return !!props.item.children && !!props.item.children?.length
})

const activeChild = ref(null)
const updateActiveItem = (key) => {
    activeChild.value = key
}

const activeKeys = inject('activeKeys')
const setActiveKeys = inject('setActiveKeys')
const deleteActiveKeys = inject('deleteActiveKeys')

watchEffect(() => {
    const currentDepActiveKey = activeKeys[props.depth]
    if (currentDepActiveKey) {
        if (currentDepActiveKey === props.item.key) {
            expanded.value = true
        }
    }
})

watch(
    () => props.activeKey,
    () => {
        if (props.accordion && props.activeKey === props.item.key) {
            const depth = props.depth
            setActiveKeys(depth, props.activeKey)
            const keys = toRaw(activeKeys)
            for (const key in keys) {
                if (key > depth) {
                    deleteActiveKeys(key)
                }
            }
        }
        if (props.accordion && props.item.key !== props.activeKey) {
            expanded.value = false
        }
    }
)
</script>

<template>
    <li :class="[
        'list-item',
        expanded && 'list-item--expanded'
    ]">
        <div :class="[
        'list-item__text',
        expanded && 'list-item__text--expanded'
    ]" @click.prevent.stop="toggleItem">
            <div v-if="isExpandable" :class="[expanded && 'list-item__arrow--expand']">▼</div>
            {{ props.item.text }}
        </div>
        <div v-if="isExpandable" :class="[
        'list-item__children',
        expanded && 'list-item__children--expanded'
    ]">
            <ListItem v-for="child in props.item.children" :key="child.key" :item="child" :accordion="props.accordion"
                :active-key="activeChild" :depth="props.depth + 1" @active-item="updateActiveItem" />
        </div>
    </li>
</template>

<style lang="scss" scoped>
.list-item {
    position: relative;
    padding: 4px 4px 4px 16px;
    display: block;
    list-style: none;
    cursor: pointer;
    text-align: left;

    &--expanded {
        background-color: lightgray;
    }

    &__text {
        display: flex;
        gap: 4px;

        &:hover {
            color: black;
        }

        &--expanded {
            color: black;
        }
    }

    &__children {
        height: 0;
        opacity: 0;
        transition: height 1s ease-out, opacity 0.4s linear;

        &--expanded {
            height: max-content;
            opacity: 1;
        }
    }

    &__arrow--expand {
        transform: rotate(-180deg);
    }
}
</style>