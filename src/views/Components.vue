<template>
    <main>
        <section class="space">
            <div class="limiter inner-layout">
                <div class="sidebar">
                    <h2>Variables</h2>
                    <div class="flex vertical gap-1">
                        <h3>Colors (type color)</h3>
                        <div class="grid margin-bottom-2">
                            <Swatch title="--color-primary" background="rgb(var(--color-primary))" color="rgb(var(--color-on-primary))" />
                            <Swatch title="--color-accent" background="rgb(var(--color-accent))" color="rgb(var(--color-on-accent))" />
                            
                            <Swatch title="--color-heading" background="rgb(var(--color-heading))" color="rgb(var(--color-background))" />
                            <Swatch title="--color-text" background="rgb(var(--color-text))" color="rgb(var(--color-background))" />
                            
                            <Swatch title="--color-border" background="rgb(var(--color-border))" color="rgb(var(--color-heading))" />
                            <Swatch title="--color-border-disabled" background="rgb(var(--color-border-disabled))" color="rgb(var(--color-heading))" />
                            
                            <Swatch title="--color-background" background="rgb(var(--color-background))" color="rgb(var(--color-heading))" />
                            <Swatch title="--color-background-soft" background="rgb(var(--color-background-soft))" color="rgb(var(--color-heading))" />
                            <Swatch title="--color-background-disabled" background="rgb(var(--color-background-disabled))" color="rgb(var(--color-heading))" />
                            
                            <Swatch title="--color-info" background="rgb(var(--color-info))" color="white" />
                            <Swatch title="--color-success" background="rgb(var(--color-success))" color="white" />
                            <Swatch title="--color-warning" background="rgb(var(--color-warning))" color="rgb(var(--color-background))" />
                            <Swatch title="--color-error" background="rgb(var(--color-error))" color="white" />
                        </div>
                        <h3>Border Radius (type length)</h3>
                        <div class="grid margin-bottom-2">
                            <RadiusCard title="S – 4px" radius="var(--radius-s)"/>
                            <RadiusCard title="M – 8px" radius="var(--radius-m)" />
                            <RadiusCard title="L – 12px" radius="var(--radius-l)"/>
                            <RadiusCard title="XL – 16px" radius="var(--radius-xl)" />
                        </div>
                    </div>
                </div>
                <div class="content">
                    <div class="flex vertical gap-1">
                        <h2>Components</h2>
                        <h3>Buttons</h3>
                        <pi-button label="Open Popup" @click="$refs.test.open()" />
                        <h3>Inputs</h3>
                        <pi-input label="Your Address" clearable icon-left="location_on" />
                        <h3>Toggles</h3>
                        <pi-toggle type="checkbox" label="Checkbox" />
                        <pi-toggle type="switch" label="Switch" />
                        <h3>Loader</h3>
                        <pi-loader type="spinner"/>
                        <pi-popup ref="test" backdrop-color="#000000cc">
                            <p>test</p>
                        </pi-popup>
                        <h3>Picker</h3>
                        <icon-picker>
                            <button>Open Picker</button>
                        </icon-picker>
                        <pi-colorpicker :swatches="swatches">
                            <button>Open Colopicker</button>
                        </pi-colorpicker>
                    </div>
                </div>
            </div>
        </section>
    </main>
</template>

<script setup>
    import PiButton from '@/components/library/Button.vue'
    import PiInput from '@/components/library/Input.vue'
    import PiToggle from '@/components/library/Toggle.vue'
    import PiLoader from '@/components/library/Loader.vue'
    import PiPopup from '@/components/library/Popup.vue'
    import PiColorpicker from '@/components/library/Colorpicker.vue'

    import IconPicker from '@/components/IconPicker.vue'
    import Swatch from '@/components/Swatch.vue'
    import RadiusCard from '@/components/RadiusCard.vue'

    import swatches from '@/components/data/flat_ui_colors.swatches.json'
</script>

<style lang="sass" scoped>
    .space
        padding-block: 10rem

    .inner-layout
        display: flex
        gap: 4rem

        h2
            padding-bottom: .5rem
            margin-bottom: 1rem
            border-bottom: 2px solid rgb(var(--color-border))

        .sidebar
            width: 350px

        .content
            flex: 1

    .grid
        display: grid
        grid-template-columns: 1fr
        gap: 1rem
</style>