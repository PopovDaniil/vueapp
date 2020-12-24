<template>
    <label>{{label}}:
        <input type="range" v-model="val" :min="min" :max="max" :style="style" @input="this.$emit('change',this.val)">
        {{val}}
    </label>
</template>

<script>
export default {
    props: {
        value: Number,
        min: Number,
        max: Number,
        label: String,
        size: Number
    },
    emits: ["change"],
    data() {
        return {
            val: this.value,
            style: {
                width: `${this.size}px`
            }
        }
    }
}
</script>