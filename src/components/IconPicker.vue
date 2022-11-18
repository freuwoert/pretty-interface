<template>
    <div class="icon-picker">
        <div class="icon" v-for="icon in icons" :key="icon" @click="selectIcon(icon)">{{icon}}</div>
    </div>
</template>

<script setup>
    const icons = [
        'home',
        'search',
        'person',
        'person_add',
        'person_remove',
        'check_circle',
        'chevron_left',
        'chevron_right',
    ]

    const selectIcon = (icon) => {
        $emit('select', icon)
    }
</script>

<style lang="sass" scoped>
    .icon-picker
        display: grid
        grid-template-columns: repeat(auto-fill, minmax(2.5rem, 1fr))
        gap: .5rem
        padding: .5rem
        border-radius: var(--radius-m)
        background: rgb(var(--color-background))
        border: 1px solid rgb(var(--color-border))
        max-height: 50vh
        overflow-y: auto

        .icon
            display: flex
            justify-content: center
            align-items: center
            font-size: 1.5rem
            line-height: 1
            aspect-ratio: 1
            border-radius: var(--radius-s)
            background: rgb(var(--color-background-soft))
            font-family: var(--font-icon)
            cursor: pointer
            transition: all 50ms ease-in-out

            &:hover
                color: rgb(var(--color-primary))
</style>