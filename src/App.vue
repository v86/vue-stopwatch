<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/no-time.gif" width="25%" />

    <div class="control">
      <label class="label" for="multiplier"
        >Multiplikator/Anzahl Personen:</label
      >&emsp;<input
        v-model="multiplier"
        name="multiplier"
        id="multiplier"
        type="number"
        size="2"
        class="input"
      />
      <br />
      <small
        >(Kann <u>jederzeit</u> geändert werden und wirkt sich auf laufende
        Uhren aus)</small
      >
    </div>

    <Stopwatch
      v-for="clock in watches"
      :key="clock.ctag"
      :multiplier="multiplier"
    />

    <button class="button button--add" @click="watches.push({})">
      + Stoppuhr
    </button>
  </div>
</template>

<script>
import Stopwatch from "./components/Stopwatch";

export default {
  name: "App",
  components: {
    Stopwatch,
  },
  data() {
    return {
      multiplier: 9,
      watches: [],
    };
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.control {
  margin-bottom: 1em;
  padding-bottom: 1em;
}
</style>
