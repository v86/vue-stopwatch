<template>
  <div class="timer">
    <div class="timer__element timer__control">
      <transition name="fade" mode="out-in">
        <button @click="start" v-if="!interval">Start</button>
        <button @click="stop" v-else>Stop</button>
      </transition>
      <button @click="reset">Reset</button>
    </div>
    <div :class="['timer__element', 'clock', { running: interval }]">
      <input v-model="ctag" type="text" class="ctag" size="4" />
      <span v-if="begin" class="clock__display">
        <span class="clock__start">({{ renderStart }})</span>
        <span class="clock__multiplier">&times;{{ multiplier }}</span>
        <span class="clock__clock">&rarr; {{ renderClock }}h</span>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    multiplier: {
      default: 8,
    },
  },
  data() {
    return {
      begin: null,
      clock: 0,
      delay: 10000, //-> 10 secs
      interval: null,
      offset: null,
      ctag: "TAG",
      step: 1000 * 60 * 15, //-> 15 mins
    };
  },
  methods: {
    start() {
      if (!this.interval) {
        this.offset = Date.now();
        this.begin = new Date();

        this.interval = setInterval(this.update, this.delay);
      }
    },

    stop() {
      if (this.interval) {
        clearInterval(this.interval);
        this.interval = null;
      }
    },

    reset() {
      this.clock = 0;
    },

    update() {
      this.clock += this.delta() * this.multiplier;
    },

    delta() {
      let now = Date.now(),
        delta = now - this.offset;

      this.offset = now;
      return delta;
    },
  },
  computed: {
    renderClock() {
      return Math.round(this.clock / this.step) / 4; // rounded to quarter hours
    },
    renderStart() {
      return this.begin.toLocaleTimeString("de-DE", {
        hour: "2-digit",
        minute: "2-digit",
      });
    },
  },
};
</script>

<style lang="scss">
.timer {
  margin: 0 2em 1em;
}

.timer__element {
  display: inline-block;
}

.running {
  position: relative; // for ::after positioning
}

.running::after {
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' style='margin:auto;background:%23fff' viewBox='0 0 100 100' preserveAspectRatio='xMidYMid' display='block'%3E%3Cpath fill='%23e15b64' d='M50 14c19.85 0 36 16.15 36 36S69.85 86 50 86 14 69.85 14 50s16.15-36 36-36m0-4c-22.091 0-40 17.909-40 40s17.909 40 40 40 40-17.909 40-40-17.909-40-40-40z'/%3E%3Cpath fill='%23f8b26a' d='M52.78 42.506a.67.67 0 01-.428-.603L52.269 40l-.931-21.225a1.339 1.339 0 00-2.676.002L47.731 40l-.083 1.901a.669.669 0 01-.428.604c-.075.028-.146.063-.22.093V44h6v-1.392c-.075-.031-.143-.073-.22-.102z'%3E%3CanimateTransform attributeName='transform' type='rotate' repeatCount='indefinite' values='0 50 50;360 50 50' keyTimes='0;1' dur='0.4166666666666667s'/%3E%3C/path%3E%3Cpath fill='%23f8b26a' d='M58.001 48.362c-.634-3.244-3.251-5.812-6.514-6.391-3.846-.681-7.565 1.35-9.034 4.941a1.17 1.17 0 01-1.013.744l-15.149.97a1.373 1.373 0 00-1.285 1.383c0 .722.564 1.321 1.283 1.363l15.153.971c.447.027.834.312 1.011.744A8.13 8.13 0 0050 58.16a8.136 8.136 0 006.301-2.975c1.557-1.889 2.177-4.377 1.7-6.823zM50 53.06c-1.688 0-3.06-1.373-3.06-3.06s1.373-3.06 3.06-3.06 3.06 1.373 3.06 3.06-1.372 3.06-3.06 3.06z'%3E%3CanimateTransform attributeName='transform' type='rotate' repeatCount='indefinite' values='0 50 50;360 50 50' keyTimes='0;1' dur='1.6666666666666667s'/%3E%3C/path%3E%3C/svg%3E");
  content: " ";
  display: inline-block;
  height: 1.125rem;
  position: absolute;
  width: 1.125rem;
}
</style>
