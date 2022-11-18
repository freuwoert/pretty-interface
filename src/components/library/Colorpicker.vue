<template>
    <div class="pi-colorpicker pi-container">
        <div class="sb-container">
            <div class="colorpanel" :style="`background: ${hueColorString};`"></div>
            <div class="handle" :style="`left: ${color.saturation * 100}%; top: ${(1 - color.brightness) * 100}%; background: ${fullColorString};`"></div>
            <area-slider class="bar" v-model:x="color.saturation" :y="1 - color.brightness" @update:y="color.brightness = 1 - $event"/>
        </div>

        <div class="slider-layout">
            <button class="eyedropper-toggle">colorize</button>
            <div class="slider-wrapper hue">
                <div class="background"></div>
                <div class="handle-wrapper">
                    <div class="handle" :style="`left: ${color.hue * 100}%; background: ${hueColorString};`"></div>
                </div>
                <area-slider class="area" :padding="8" v-model:x="color.hue" />
            </div>
            
            <div class="slider-wrapper alpha">
                <div class="background" :style="`background: ${alphaColorGradientString}; color: ${fullColorString};`"></div>
                <div class="handle-wrapper">
                    <div class="handle" :style="`left: ${color.alpha * 100}%;`"></div>
                </div>
                <area-slider class="area" :padding="8" v-model:x="color.alpha" />
            </div>
        </div>

        <div class="output-layout">
            <select v-model="outputMode">
                <option value="hex">Hex</option>
                <option value="rgb">RGB</option>
                <option value="hsl">HSL</option>
                <option value="hsb">HSB</option>
            </select>

            <div class="sub-layout">
                <input type="text" :value="fullHexColorString">
                
                <input type="number" class="alpha" :value="Math.round(color.alpha * 100)" min="0" max="100" @input="event => color.alpha = event.target.value / 100">
            </div>
        </div>
    </div>
</template>

<script>
    import AreaSlider from './parts/AreaSlider.vue'

    export default {
        data() {
            return {
                color: {
                    hue: 0,
                    saturation: 1,
                    brightness: 1,
                    alpha: 1,
                },
                outputMode: 'hex',
            }
        },

        computed: {
            hueColorString() {
                return `hsl(${this.color.hue * 360}, 100%, 50%)`
            },

            alphaColorGradientString() {
                let color = this.hsb2hsl(this.color)
                return `linear-gradient(to right, hsl(${color.hue * 360}deg, ${color.saturation * 100}%, ${color.lightness * 100}%, 0), hsl(${color.hue * 360}deg, ${color.saturation * 100}%, ${color.lightness * 100}%, 1))`
            },

            fullColorString() {
                let color = this.hsb2hsl(this.color)
                return `hsl(${color.hue * 360}deg ${color.saturation * 100}% ${color.lightness * 100}%)`
            },

            fullHexColorString() {
                return this.rgb2hex(this.hsb2rgb(this.color))
            },
        },

        methods: {
            hsb2hsl(color) {
                let { hue, saturation, brightness } = color
                
                let lightness = (2 - saturation) * brightness / 2

                if (lightness != 0) {
                    if (lightness == 1) {
                        saturation = 0
                    } else if (lightness < .5) {
                        saturation = saturation * brightness / (lightness * 2)
                    } else {
                        saturation = saturation * brightness / (2 - lightness * 2)
                    }
                }

                return {
                    hue,
                    saturation,
                    lightness,
                    alpha: color.alpha,
                }
            },

            hsb2rgb(color) {
                let { hue, saturation, brightness } = color

                let r, g, b

                if (saturation == 0)
                {
                    r = g = b = brightness
                }
                else
                {
                    let h = hue * 360 / 60
                    let i = Math.floor(h)
                    let f = h - i
                    let p = brightness * (1 - saturation)
                    let q = brightness * (1 - saturation * f)
                    let t = brightness * (1 - saturation * (1 - f))

                    switch (i) {
                        case 0:
                            r = brightness
                            g = t
                            b = p
                            break
                        case 1:
                            r = q
                            g = brightness
                            b = p
                            break
                        case 2:
                            r = p
                            g = brightness
                            b = t
                            break
                        case 3:
                            r = p
                            g = q
                            b = brightness
                            break
                        case 4:
                            r = t
                            g = p
                            b = brightness
                            break
                        case 5:
                            r = brightness
                            g = p
                            b = q
                            break
                        default:
                            r = brightness
                            g = p
                            b = q
                    }
                }

                return {
                    red: r,
                    green: g,
                    blue: b,
                    alpha: color.alpha,
                }
            },

            rgb2hex(color) {
                let { red, green, blue } = color
                
                let r = Math.round(red * 255).toString(16).padStart(2, '0')
                let g = Math.round(green * 255).toString(16).padStart(2, '0')
                let b = Math.round(blue * 255).toString(16).padStart(2, '0')

                return `${r}${g}${b}`.toUpperCase()
            },
        },

        components: {
            AreaSlider,
        },
    }
</script>

<style lang="sass" scoped>
    .pi-container.pi-colorpicker
        background: rgb(var(--color-background))
        border-radius: var(--radius-m)
        padding: .5rem 0
        border: 1px solid rgb(var(--color-border))
        width: 320px
        display: flex
        flex-direction: column

        .handle
            height: 14px
            width: 14px
            border-radius: 50%
            border: 2px solid white
            box-shadow: 0 1px 1px 2px #00000020
            position: absolute
            z-index: 1
            pointer-events: none

        .sb-container
            display: flex
            aspect-ratio: 1
            position: relative
            border-top: 1px solid rgb(var(--color-border))
            border-bottom: 1px solid rgb(var(--color-border))

            .handle
                transform: translate(-50%, -50%)

            .bar
                height: 100%
                width: 100%
                position: absolute
                top: 0
                left: 0

            .colorpanel
                width: 100%
                height: 100%
                position: relative

                &:after
                    content: ''
                    position: absolute
                    top: 0
                    left: 0
                    width: 100%
                    height: 100%
                    pointer-events: none
                    background: linear-gradient(to bottom, rgb(0, 0, 0, 0), rgb(0, 0, 0, 1))

                &:before
                    content: ''
                    position: absolute
                    top: 0
                    left: 0
                    width: 100%
                    height: 100%
                    background: linear-gradient(to right, rgb(255, 255, 255, 1), rgb(255, 255, 255, 0))
                    pointer-events: none

        .slider-wrapper
            --height: 16px
            width: 100%
            height: var(--height)
            border-radius: calc(var(--height) / 2)
            position: relative

            &.hue
                background: red

                .background
                    background: linear-gradient(to right, red, yellow, lime, cyan, blue, magenta, red)

            &.alpha
                background-color: white
                background-image: linear-gradient(45deg, #b4b4b4 25%, transparent 25%), linear-gradient(-45deg, #b4b4b4 25%, transparent 25%), linear-gradient(45deg, transparent 75%, #b4b4b4 75%), linear-gradient(-45deg, transparent 75%, #b4b4b4 75%)
                background-size: calc(var(--height) / 2) calc(var(--height) / 2)
                background-position: 0 0, 0 calc(var(--height) / 4), calc(var(--height) / 4) calc(var(--height) / 4 * -1), calc(var(--height) / 4 * -1) 0px

                .background::after
                    content: ''
                    position: absolute
                    top: 0
                    left: 100%
                    bottom: 0
                    width: calc(var(--height) / 2)
                    background: currentColor
                    border-radius: 0 calc(var(--height) / 2) calc(var(--height) / 2) 0

            .background
                position: absolute
                top: 0
                bottom: 0
                left: calc(var(--height) / 2)
                right: calc(var(--height) / 2)

            .handle-wrapper
                position: absolute
                top: 0
                bottom: 0
                left: calc(var(--height) / 2)
                right: calc(var(--height) / 2)

                .handle
                    top: 1px
                    transform: translateX(-50%)

            .area
                position: absolute
                top: 0
                bottom: 0
                left: 0
                right: 0

        .slider-layout
            display: grid
            grid-template-columns: auto 1fr
            grid-template-rows: 1fr 1fr
            grid-template-areas: 'eyedropper hue' 'eyedropper alpha'
            align-items: center
            padding: 1rem
            gap: 1rem
            border-bottom: 1px solid rgb(var(--color-border))

            .eyedropper-toggle
                grid-area: eyedropper
                aspect-ratio: 1
                width: 2.5rem
                display: flex
                align-items: center
                justify-content: center
                font-family: var(--font-icon)
                font-size: 1.4rem
                line-height: 1
                color: rgb(var(--color-text))
                background: rgb(var(--color-background))
                border-radius: var(--radius-s)
                padding: 0
                border: none
                cursor: pointer
                user-select: none

                &:hover
                    background: rgb(var(--color-background-soft))
                    color: rgb(var(--color-heading))

        .output-layout
            display: flex
            align-items: center
            padding: 1rem
            gap: 1rem
            border-bottom: 1px solid rgb(var(--color-border))

            .sub-layout
                flex: 1
                display: flex
                align-items: center
                gap: 1rem
                
                input
                    width: 0
                    flex: 1

                .alpha
                    width: 3rem
                    flex: none
                    
                    &::-webkit-outer-spin-button,
                    &::-webkit-inner-spin-button
                        -webkit-appearance: none
                        margin: 0
                        appearance: none

            select, input
                border: none
                background: rgb(var(--color-background-soft))
                color: rgb(var(--color-text))
                font-family: var(--font-text)
                padding: 0 .5rem
                min-width: 0
                margin: 0
                border-radius: var(--radius-s)
                height: 2rem
                flex: none

            select
                width: 4rem
</style>