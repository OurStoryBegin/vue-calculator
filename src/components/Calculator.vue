<template>
  <div class="calculator">
    <div class="display"><p>{{ result || 0}}</p></div>
    <div @click="clear" class="nan">AC</div>
    <div class="backspace nan"><b-icon-backspace></b-icon-backspace></div>
    <div class="percentage nan">%</div>
    <div class="nan">/</div>
    <div @click="append('7')" >7</div>
    <div @click="append('8')">8</div>
    <div @click="append('9')">9</div>
    <div class="nan">*</div>
    <div @click="append('4')">4</div>
    <div @click="append('5')">5</div>
    <div @click="append('6')">6</div>
    <div class="nan">-</div>
    <div @click="append('1')">1</div>
    <div @click="append('2')">2</div>
    <div @click="append('3')">3</div>
    <div class="nan">+</div>
    <div class="transform"><b-icon-arrow-repeat></b-icon-arrow-repeat></div>
    <div @click="append('0')">0</div>
    <div @click="append('.')" class="dot">.</div>
    <div class="nan">=</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      result: ''
    }
  },
  methods: {
    clear() {
      this.result = ''
    },
    append(operand) {
      this.result += operand
    }

  }
}
</script>


<style scoped>
  .calculator {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-auto-rows: minmax(80px, auto);
    width: 420px;
    margin: 0 auto;
    background-color: #eee;
    font-size: 2rem;
    border-radius: 5px;
  }

  .display {
    grid-column: 1 / 5;
    grid-row: 1 / 3;
    background-color: #aaa;
    border-radius: 5px 5px 0 0;
    margin-bottom: 1rem;
    position: relative;
  }
  .display p {
    position: absolute;
    bottom: 0;
    right: 0;
    text-align: center;
  }

  .nan {
    /* background-color: orange; */
    color: orange;
  }

  .transform {
    cursor: pointer;
  }


</style>
