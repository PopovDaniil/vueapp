<template>
    <label>{{label}}:
    <input readonly :value="value.toFixed(this.precision)" :style="style"/>
    {{unit}}
    </label>
</template>

<script>
export default {
    props: {
        value: Number,
        label: String,
        width: {
            type: Number,
            default: 50
        },
        precision: {
            type: Number,
            default: 2
        },
        unit: String
    },
    data() {
        return {
            style: {
                width: `${this.width}px`
            }
        }
    },
}
</script>