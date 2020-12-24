<template>
    <button @click="this.switch">{{text}}</button>
</template>

<script>
export default {
    props: {
        onText: {
            type: String,
            default: "Вкл."
        },
        offText: {
            type: String,
            default: "Выкл."
        },
        val: Boolean
    },
    emits: ['click'],
    data() {
        return {
            state: this.val
        }
    },
    methods: {
        switch() {
            this.state = !this.state;
            this.$emit('click');
        }
    },
    computed: {
        text: {
            get() {
                return this.state ? this.onText : this.offText
            }
        },
    }
}
</script>