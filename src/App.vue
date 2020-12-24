<template>
  <engine :value="this.engine" @change="engineChanged"></engine>
  <display :value="speed" label="Скорость" unit="м/с" />
  <display :value="acceleration" label="Ускорение" unit="м/с2" />
  <display :value="friction" label="Трение" unit="м/с2" />
  <slider
    :min="0"
    :max="10"
    :value="0"
    :label="'Тяга'"
    :size="300"
    @change="throttleChanged"
  />
  <slider
    :min="0"
    :max="10"
    :value="0"
    :label="'Тормоз'"
    :size="300"
    @change="brakeChanged"
  />
</template>

<script>
let speedInterval;

Number.prototype.roundDec = function (pr) {
  return Math.round(this * (10**pr)) / (10**pr);
};
Number.prototype.getSign = function () {
  return this >= 0 ? 1 : -1;
}
Number.prototype.isInInterval = function (a,b) {
  return ((this >= a) && (this <= b)) || ((this >= b) && (this <= a));
}
Number.prototype.isCloseTo = function(tg,dt) {
  return this.isInInterval(tg + dt, tg - dt);
}

import Display from "./components/Elements/Display.vue";
import Slider from "./components/Elements/Slider.vue";
import Engine from "./components/Engine.vue";

export default {
  components: { Engine, Display, Slider },
  name: "App",
  data() {
    return {
      engine: true,
      moving: {
        speed: 0,
        acceleration: 0,
        roadFriction: 0.03,
        brakeFriction: 0
      },
    };
  },
  methods: {
    engineChanged(state) {
      this.engine = state;
      this.moving.acceleration = 0;
    },
    throttleChanged(val) {
      if (this.engine) this.moving.acceleration = val / 100;
    },
    brakeChanged(val) {
      this.moving.brakeFriction = val / 100;
    }
  },
  watch: {
    acceleration(newAcc) {
      if (newAcc > 0) {
        clearInterval(speedInterval);
        speedInterval = setInterval(() => {
          this.speed += this.acceleration;
        }, 10);
      }
    },
  },
  computed: {
    acceleration() {
      return (this.moving.acceleration - this.friction);
    },
    friction() {
      const sp = this.moving.speed;
      const fr = this.moving.roadFriction + this.moving.brakeFriction;
      const sign = sp.getSign();
      if (sp.isCloseTo(0,0.01)) {
        return 0;
      } else {
        return fr * sign;
      }
    },
    speed: {
      get() {
        return this.moving.speed.roundDec(2)
      },
      set(val) {
        if (val.isCloseTo(0,this.moving.roadFriction)) {
          this.moving.speed = 0
        }  else
        this.moving.speed = val;
      },
    },
  },
};
</script>

<style>
label {
  display: block;
  margin: 5px;
}
</style>
