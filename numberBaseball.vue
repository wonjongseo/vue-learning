<template>
  <div id="root">
    <h1>{{ result }}</h1>
    <!-- v-on: == @ -->
    <!-- @submit.prevent == e.preventDefault() -->
    <div>{{ answer }}</div>
    <form @submit.prevent="onSumbmitForm">
      <input ref="valueRef" minlength="4" maxlength="4" v-model="value" />
      <button type="submit">Click</button>
    </form>
    <div>{{ result }}</div>
    <div>Tires : {{ tries.length }}</div>
    <ul>
      <div>
        <li v-for="(v, key) in tries" :key="key">
          <div>Try number[{{ key + 1 }}] : {{ v.try }}</div>
          <div>{{ v.result }}</div>
        </li>
      </div>
    </ul>
  </div>
</template>

<script>
const getNumber = () => {
  const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9];
  const returns = [];
  for (let i = 0; i < 4; i++) {
    const chosne = numbers.splice(Math.floor(Math.random() * (9 - i)), 1)[0];
    returns.push(chosne);
  }
  return returns;
};

export default {
  data() {
    return {
      answer: getNumber(),
      value: "",
      result: "Testing",
      tries: [],
    };
  },
  methods: {
    onSumbmitForm(e) {
      if (this.value === this.answer.join("")) {
        this.tries.push({
          try: this.value,
          result: "good",
        });
        (this.result = "good"), (this.tries = []);
        return;
      } else if (this.tries.length >= 9) {
        this.result = "fail";
        this.tries = [];
        return;
      } else {
        let strike = 0;
        let ball = 0;

        const valueArrList = this.value.split("").map((v) => parseInt(v));

        const valueArr = this.value.split("");

        for (let i = 0; i < valueArr.length; i++) {
          if (this.answer[i] === parseInt(valueArr[i])) {
            strike++;
          } else {
            for (let j = 0; j < valueArr.length; j++) {
              if (j == i) continue;

              //   this.answer.includes(values[i]);
              if (this.answer[i] === parseInt(valueArr[j])) {
                ball++;
                break;
              }
            }
          }
        }
        this.tries.push({
          try: this.value,
          result: `${strike} Strike, ${ball} Ball`,
        });
      }

      this.value = "";
      this.$refs.valueRef.focus();
    },
  },
};
</script>

<style></style>
