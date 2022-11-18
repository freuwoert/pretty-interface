<template>
    <div class="pi-container" ref="container" @mousedown.prevent="handleMouseDown" @drag.prevent.stop></div>
</template>

<script setup>
    import { ref, defineEmits, watch } from 'vue'

    const props = defineProps({
        x: {
            type: Number,
            default: 0,
        },
        y: {
            type: Number,
            default: 0,
        },
        padding: {
            type: Number,
            default: 0,
        },
    })

    const emits = defineEmits(['update:x', 'update:y'])



    const position = ref({
        x: 0,
        y: 0,
    })
    const container = ref(null)
    const isDragging = ref(false)

    const handleMouseDown = (e) => {
        isDragging.value = true
        window.addEventListener('mousemove', handleMouseMove)
        window.addEventListener('mouseup', handleMouseUp)
    }

    const handleMouseMove = (e) => {
        let rect = container.value.getBoundingClientRect()

        // Calculate the position of the mouse relative to the container respecting the padding
        let x = (e.clientX - rect.left - props.padding) / (rect.width - props.padding * 2)
        let y = (e.clientY - rect.top - props.padding) / (rect.height - props.padding * 2)

        position.value = {
            x: Math.max(0, Math.min(1, x)),
            y: Math.max(0, Math.min(1, y)),
        }
        
        emits('update:x', position.value.x)
        emits('update:y', position.value.y)
    }

    const handleMouseUp = (e) => {
        isDragging.value = false
        window.removeEventListener('mousemove', handleMouseMove)
        window.removeEventListener('mouseup', handleMouseUp)
    }



    watch(() => props.x, (x) => {
        if (!isDragging.value) position.value.x = Math.max(0, Math.min(1, x))
    })

    watch(() => props.y, (y) => {
        if (!isDragging.value) position.value.y = Math.max(0, Math.min(1, y))
    })
</script>