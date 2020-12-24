<template>
  <div class="indicator" :class="{on: state, off: !state}"></div>
</template>

<script>
export default {
  props: {
      state: {
          type: Boolean,
          default: false
      }
  }
};
</script>

<style>
div.indicator {
  width: 20px;
  height: 20px;
  border: 1px solid #000;
  margin: 5px;
}

div.indicator.on {
  background-color: green;
}

div.indicator.off {
  background-color: red;
}
</style>