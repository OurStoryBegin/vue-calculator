<template>
  <div class="calculator">
    <div class="display">
      <div class="history">
        <p v-for="log in formulaHistory"
            :key="log"
        >{{log}}</p>
      </div>
      <p class="formulaStyle" :class="{focusOn: !equalClicked}">{{ formula }}</p>
      <p class="resultStyle" :class="{focusOn: equalClicked}">{{formula ? '=' : ''}}{{ result || 0}}</p>
    </div>
    <div class="keyboard">
      <div @click="clear" class="nan">AC</div>
      <div @click="backspace" class="nan"><b-icon-backspace></b-icon-backspace></div>
      <div @click="percentage" class="nan">%</div>
      <div @click="divide" class="nan">&#247;</div>
      <div @click="append('7')">7</div>
      <div @click="append('8')">8</div>
      <div @click="append('9')">9</div>
      <div @click="multiple" class="nan">&#215;</div>
      <div @click="append('4')">4</div>
      <div @click="append('5')">5</div>
      <div @click="append('6')">6</div>
      <div @click="minus" class="nan">&#8722;</div>
      <div @click="append('1')">1</div>
      <div @click="append('2')">2</div>
      <div @click="append('3')">3</div>
      <div @click="add" class="nan">+</div>
      <div @click="append('0')" class="zero">0</div>
      <div @click="dot('.')" class="dot">.</div>
      <div @click="equal" class="nan">=</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // preoperand: '',
      formula: '',
      equalClicked: false,
      formulaHistory: [],
      // percentaged: false
      // operator: null,
      // operatorClicked: false,
      // formulaEndNan: false
    }
  },
  computed: {
    result() {
      let diviReg = new RegExp(String.fromCharCode(247), 'g')
      let multiReg = new RegExp(String.fromCharCode(215), 'g')
      let minusReg = new RegExp(String.fromCharCode(8722), 'g')

      let tmp = this.formula
      if(tmp.slice(-1) === '.') tmp = tmp.slice(0, -1)
      if(this.operatorEnd(tmp)) {
        tmp = tmp.slice(0, -1)
      }
      tmp = tmp.replace(diviReg, '/')
      tmp = tmp.replace(multiReg, '*')
      tmp = tmp.replace(minusReg, '-')

      let result = (Function("return " + tmp))()
      if(Number.isNaN(result) || result === Infinity) result = "Can't divide by zero"
      return result
    }
  },
  methods: {
    operatorEnd(formula) {
      let end = formula.slice(-1)
      let out = false
      switch (end) {
        case '+':
        case String.fromCharCode(247):
        case String.fromCharCode(215):
        case String.fromCharCode(8722):
          out = true;
          break;
      }
      return out;
    },
    clear() {
      this.formula = ''
      this.formulaHistory = []
      // this.equalClicked = false
      // this.operatorClicked = false
      // this.formulaEndNan = false
    },
    append(operand) {
      if(this.formula === '' && operand === '0') {
        this.formula = ''
      } else {
        this.formula += operand
      }
      this.equalClicked = false
    },
    dot(dot) {
      if(this.equalClicked) {
        this.formulaHistory.push(this.formula + '=' + this.result)
        this.formula = '0.'
      }
      if(this.formula === '') {
        this.formula = '0.'
      }
      if(this.formula.indexOf('.') === -1) {
        this.formula += dot
      }
      this.equalClicked = false
    },
    backspace() {
      this.formula = this.formula.slice(0, -1)
    },
    percentage() {
      if(this.operatorEnd(this.formula)) return;
      if(this.equalClicked) {
        this.formulaHistory.push(this.formula + '=' + this.result)
      }
      // console.log(this.result)
      let tmp = this.result / 100
      // console.log(tmp)
      this.formula = String(tmp)
      // if(!this.operatorEnd(this.formula))
      //   this.formula += '/100'
      // console.log(this.result, this.formula)
      // let tmp = this.result
      // this.formula = String(tmp)
      this.equalClicked = false
    },
    divide() {
      if(this.equalClicked) {
        this.formulaHistory.push(this.formula + '=' + this.result)
        this.formula = String(this.result)
      }
      if(this.formula === '') {
        this.formula = '0' + String.fromCharCode(247)
      } else if(this.operatorEnd(this.formula)) {
        this.formula = this.formula.slice(0, -1) + String.fromCharCode(247)
      } else {
        this.formula += String.fromCharCode(247)
      }
      this.equalClicked = false
    },
    multiple() {
      if(this.equalClicked) {
        this.formulaHistory.push(this.formula + '=' + this.result)
        this.formula = String(this.result)
      }
      if(this.formula === '') {
        this.formula = '0' + String.fromCharCode(215)
      } else if(this.operatorEnd(this.formula)) {
        this.formula = this.formula.slice(0, -1) + String.fromCharCode(215)
      } else {
        this.formula += String.fromCharCode(215)
      }
      this.equalClicked = false
    },
    minus() {
      if(this.equalClicked) {
        this.formulaHistory.push(this.formula + '=' + this.result)
        this.formula = String(this.result)
      }
      if(this.formula === '') {
        this.formula = '0' + String.fromCharCode(8722)
      } else if(this.operatorEnd(this.formula)) {
        this.formula = this.formula.slice(0, -1) + String.fromCharCode(8722)
      } else {
        this.formula += String.fromCharCode(8722)
      }
      this.equalClicked = false
    },
    add() {
      if(this.equalClicked) {
        this.formulaHistory.push(this.formula + '=' + this.result)
        this.formula = String(this.result)
      }
      if(this.formula === '') {
        this.formula = '0+'
      } else if(this.operatorEnd(this.formula)) {
        this.formula = this.formula.slice(0, -1) + '+'
      } else {
        this.formula += '+'
      }
      this.equalClicked = false
    },
    equal() {
      this.equalClicked = true
    }
  }
}
</script>


<style scoped>
  .calculator {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-auto-rows: minmax(90px, auto);
    /* justify-content: center; */
    /* align-items: center; */
    width: 420px;
    margin: 0 auto;
    background-color: #eee;
    font-size: 1.8rem;
    border-radius: 5px;
  }

  .display {
    grid-column: 1 / 5;
    grid-row: 1 / 4;
    /* display: grid;
    grid-template-columns: 1fr;
    grid-auto-rows: 30px; */
    background-color: #332d2d;
    border-radius: 5px 5px 0 0;
    position: relative;
    overflow: auto;
    width: 420px;
    height: 260px;
    /* justify-content: right; */
    /* align-items: right; */
    text-align: right;
    padding-right: 30px;
  }
  .history {
    position: absolute;
    bottom: 135px;
    right: 30px;
    font-size: 1rem;
    color: #9E9E9E;
    overflow: auto;
    width: 420px;
    height: 100px;
    text-align: right;
  }
  .history > p {
    margin-bottom: 2px;
  }
  /* .history > p:last-child {
    scroll-snap-align: end;
  } */
    .formulaStyle {
      position: absolute;
      bottom: 60px;
      right: 30px;
      text-align: center;
      color: #9E9E9E;
    }
    .resultStyle {
      position: absolute;
      bottom: 0;
      right: 30px;
      text-align: center;
      color: #9E9E9E;
    }

  .keyboard {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-auto-rows: minmax(80px, auto);
    width: 420px;
    align-items: center;
  }
  .keyboard > div:hover {
    background-color: #888;
    padding: 18px 18px 18px 18px;
    border-radius: 50%;
  }

  .zero {
    grid-column: 1 / 3;
  }
  .zero:hover {
    border-radius: 0px;
    padding: 0;
  }

  .nan {
    /* background-color: orange; */
    color: orange;
  }

  .focusOn {
    color: #fff;
    font-size: 2.5rem;
  }
  .clicked {
    background-color: #66c3d3;
  }

</style>
