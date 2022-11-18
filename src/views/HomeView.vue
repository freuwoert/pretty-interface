<template>
    <main>
        <section id="hero">
            <video src="/videos/bg.mp4" loop autoplay muted></video>
            <h1>Simple<span>.</span> Clean<span>.</span> Robust<span>.</span></h1>
            <p>
                A UI library for the modern web
            </p>
        </section>
        <section class="space">
            <div class="limiter">
                <h2>Easy to install and use</h2>
                <p>
                    PI is designed to be easy to implement and use.
                </p>
            </div>
        </section>
        <section class="space">
            <div class="limiter">
                <h2>Accessable for everyone</h2>
                <p>

                </p>
            </div>
        </section>
        <section class="space">
            <div class="limiter">
                <h2>Capturing that vanilla feel</h2>
                <p>
                    UI elements that feel like they belong in the browser.
                </p>
            </div>
        </section>
        <section class="space" id="demo">
            <div class="limiter">
                <pi-input label="Your Address" clearable icon-left="location_on"/>
                <pi-button label="Open Popup" @click="$refs.test.open()"/>
                <pi-popup ref="test" backdrop-color="#000000cc">
                    <p>test</p>
                </pi-popup>
            </div>
        </section>
    </main>
</template>

<script setup>
    import PiButton from '@/components/library/Button.vue'
    import PiPopup from '@/components/library/Popup.vue'
    import PiInput from '@/components/library/Input.vue'
</script>

<style lang="sass" scoped>
    #hero
        height: 700px
        width: 100%
        display: flex
        align-items: center
        justify-content: center
        flex-direction: column
        gap: 1rem
        padding-top: var(--height-header)
        border-bottom: 2px solid rgb(var(--color-background-soft))

        video
            position: absolute
            top: 0
            left: 0
            width: 100%
            height: 100%
            object-fit: cover
            object-position: center
            filter: brightness(.7) saturate(1.5) hue-rotate(-35deg)

        h1
            font-size: 5rem
            color: white
            text-shadow: 0 5px 15px rgba(0, 0, 0, .3)

            span
                font-weight: inherit
                color: rgb(var(--color-primary))

        p
            color: #ffffffcc
            font-size: 2.5rem
            text-align: center
            margin: 0
            font-family: var(--font-heading)
            text-shadow: 0 5px 15px rgba(0, 0, 0, .3)

    .space
        padding-block: 10rem
</style>