<template>
    <header>
        <div class="nav-row">
            <div class="limiter main">
                <RouterLink to="/">
                    <img src="/images/pi_logo.svg" alt="Pretty Interface Logo" title="Pretty Interface home" class="logo">
                </RouterLink>
    
                <div class="spacer"></div>
    
                <RouterLink to="/components">Components</RouterLink>
                <!-- <RouterLink to="/about">Design Guide</RouterLink> -->
                <RouterLink to="/about">Get Started</RouterLink>
                <RouterLink to="/about">Installation</RouterLink>

                <div class="divider"></div>
    
                <button class="theme-toggle" :title="`Switch to ${theme === 'dark' ? 'light theme' : 'dark theme'}`" @click="toggleTheme">
                    <div class="icon" v-show="theme === 'dark'">light_mode</div>
                    <div class="icon" v-show="theme === 'light'">dark_mode</div>
                </button>
            </div>
        </div>

        <div class="mega-menu" v-if="false">
            <div class="limiter">
                <RouterLink to="/about">
                    <img src="/images/button.svg" alt="Button" class="image">
                    <div class="text">Buttons</div>
                </RouterLink>
                <RouterLink to="/about">
                    <img src="/images/input.svg" alt="Text Input" class="image">
                    <div class="text">Inputs</div>
                </RouterLink>
                <RouterLink to="/about">
                    <img src="/images/checkbox.svg" alt="Checkbox" class="image">
                    <div class="text">Toggles</div>
                </RouterLink>
                <RouterLink to="/about">
                    <img src="/images/radio.svg" alt="Radio" class="image">
                    <div class="text">Radio Buttons</div>
                </RouterLink>
                <RouterLink to="/about">
                    <img src="/images/loader.svg" alt="Loader" class="image">
                    <div class="text">Loaders</div>
                </RouterLink>
                <RouterLink to="/about">
                    <img src="/images/popup.svg" alt="Popup" class="image">
                    <div class="text">Modals</div>
                </RouterLink>
                <RouterLink to="/about">
                    <img src="/images/datepicker.svg" alt="Datepicker" class="image">
                    <div class="text">Date picker</div>
                </RouterLink>
            </div>
        </div>
    </header>

    <RouterView />

    <footer>
        <div class="links-row"></div>
        <div class="copyright-row"></div>
    </footer>
</template>

<script setup>
    import { RouterLink, RouterView } from 'vue-router'
    import { ref, watch } from 'vue'

    const theme = ref('dark')

    const toggleTheme = () => {
        theme.value = (theme.value === 'light') ? 'dark': 'light'
    }

    // Watch for theme changes
    watch(theme, (newTheme) => {
        document.documentElement.setAttribute('data-theme', newTheme)
    }, { immediate: true })
</script>

<style lang="sass" scoped>
    header
        display: flex
        flex-direction: column
        position: fixed
        top: 0
        left: 0
        width: 100%
        z-index: 1000

        .nav-row
            display: flex
            align-content: center
            width: 100%
            background-color: rgb(var(--color-background), 70%)
            backdrop-filter: blur(10px) saturate(0%)
            border-bottom: 2px solid rgb(var(--color-primary))

        
        .main.limiter
            height: var(--height-header)
            display: flex
            align-items: center
            gap: 2rem

            .logo
                height: 1.25rem

            a
                display: flex
                padding: .5rem 1rem
                align-items: center
                color: rgb(var(--color-heading))
                text-decoration: none
                font-size: 1rem
                font-family: var(--font-heading)
                transition: color 50ms ease-in-out

                &:hover
                    color: rgb(var(--color-primary))

            .divider
                width: 1px
                height: 2rem
                background-color: rgb(var(--color-background-soft))

            .theme-toggle
                display: flex
                align-items: center
                justify-content: center
                width: 2rem
                height: 2rem
                padding: 0
                border-radius: 50%
                border: none
                cursor: pointer
                transition: all 50ms ease-in-out
                color: rgb(var(--color-heading))
                background: rgb(var(--color-background-soft))

                &:hover,
                &:focus
                    outline: none
                    background: rgb(var(--color-primary))

                .icon
                    font-family: var(--font-icon)
                    font-size: 1.1rem
                    line-height: 1

        .mega-menu
            width: 100%
            background-color: rgb(var(--color-background), 70%)
            backdrop-filter: blur(10px) saturate(0%)
            border-bottom: 2px solid rgb(var(--color-primary))

            > .limiter
                display: grid
                grid-template-columns: repeat(auto-fit, minmax(12rem, 1fr))
                gap: 3rem
                align-items: flex-start
                justify-content: space-between
                padding-block: 3rem

            a
                flex: 1
                display: flex
                flex-direction: column
                color: rgb(var(--color-heading))
                text-decoration: none
                border-radius: var(--radius-l)

                &:hover
                    background: rgb(var(--color-primary))

                    .image
                        border-color: rgb(var(--color-primary))

                .text
                    width: 100%
                    padding: .5rem 1rem
                    font-family: var(--font-heading)

                .image
                    width: 100%
                    aspect-ratio: 1.586
                    background: rgb(var(--color-background))
                    border-radius: inherit
                    border: 2px solid rgb(var(--color-background-soft))

    footer
        width: 100%
        background-color: rgb(var(--color-background))
        border-top: 2px solid rgb(var(--color-background-soft))

        .links-row
            height: 12rem

        .copyright-row
            height: 4rem
            display: flex
            align-items: center
            font-size: 0.8rem
            background: rgb(var(--color-background-soft))
</style>
