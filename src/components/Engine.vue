<template>
    <div>
        <switch-button @click="change" :val="state"/>
        <indicator :state="state"/>
    </div>
</template>

<script>
import Indicator from './Elements/Indicator.vue'
import SwitchButton from './Elements/SwitchButton'

export default {
    props: {
        value: Boolean
    },
    emits: ["change"],
    data() {
        return {
            state: this.value
        }
    },
    methods: {
        change() {
            this.state = !this.state;
            this.$emit("change",this.state);
        }
    },
    components: {
        SwitchButton,
        Indicator
    }
}
</script>