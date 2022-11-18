<template>
  <div>
    <!-- v-bind == : -->
    <div id="screen" :class="state" @click="onClickScreen">
      {{ message }}
    </div>
    <ul>
      <li v-for="(time, key) in result" :key="key">
        <div>{{ time }}ms</div>
      </li>
    </ul>
    <div>Arg time : {{ result.reduce((a, c) => a + c, 0) / result.length || 0 }} ms</div>
    <button @click="onReset">Reset</button>
  </div>
</template>

<script>
let startTime = 0;
let endTime = 0;
let timeout = null;
export default {
  data() {
    return {
      message: "Click to Start",
      state: "waiting",
      result: [],
    };
  },
  methods: {
    onReset() {
      this.result = [];
      this.state = "waiting";
    },
    onClickScreen() {
      if (this.state == "waiting") {
        this.state = "ready";
        this.message = "Click when is Green";
        timeout = setTimeout(() => {
          this.state = "now";
          this.message = "Click Now!";
          startTime = Date.now();
        }, Math.floor(Math.random() * 1000) + 1000);
      } else if (this.state == "ready") {
        // clearTimeout(timeout);
        this.state = "waiting";
        this.message = "Too Much Hurry";
      } else if (this.state == "now") {
        endTime = Date.now();

        this.state = "waiting";
        this.message = "Click to Start";
        const dif = endTime - startTime;
        console.log(this.result.length);
        this.result.push(dif);
        for (let i = 0; i < this.result.length; i++) {
          console.log(this.result[i]);
        }
      }
    },
  },
};
</script>

<style scoped>
#screen {
  width: 200px;
  height: 200px;
  display: flex;
  justify-content: center;
  align-items: center;
  user-select: none;
}

#screen.waiting {
  background-color: aqua;
}
#screen.ready {
  background-color: red;
  color: white;
}
#screen.now {
  background-color: greenyellow;
}
</style>
