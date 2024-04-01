<script setup>
import { ref, watch } from 'vue';

const dialog = ref(null)

const props = defineProps({
    closeDelay: {
        type: Number,
        default: 500
    },
    handleClose: {
        type: Function,
        default: () => { }
    },
    open: {
        type: Boolean,
        default: false
    },
})

const emit = defineEmits(['close'])

function checkClickClose(e) {
    const dimensions = dialog.value.getBoundingClientRect()
    if (
        e.clientX < dimensions.left ||
        e.clientX > dimensions.right ||
        e.clientY < dimensions.top ||
        e.clientY > dimensions.bottom
    ) {
        emit('close')
    }
}

watch(
    () => props.open,
    () => {
        if (props.open) {
            dialog.value.showModal();
            dialog.value.addEventListener('click', checkClickClose)
        } else {
            setTimeout(() => {
                dialog.value.close()
            }, props.closeDelay)
            dialog.value.removeEventListener('click', checkClickClose)
        }
    }
)
</script>

<template>
    <dialog ref="dialog" @click.native="closeDialog">
        <slot />
    </dialog>
</template>