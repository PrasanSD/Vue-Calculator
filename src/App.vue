<template>
  <div id="app">
   <div class="calculator-grid">
        <div class="output">
            <div class="previous-operand">{{ previousVal }}</div>
            <div class="current-operand">{{ currentVal }}</div>
        </div>
        <button @click="clear">AC</button>
        <button @click="sign">+/-</button>
        <button @click="percent">%</button>
        <button @click="divide">÷</button>
        <button @click="appendNumber('1')">1</button>
        <button @click="appendNumber('2')">2</button>
        <button @click="appendNumber('3')">3</button>
        <button @click="multiply">*</button>
        <button @click="appendNumber('4')">4</button>
        <button @click="appendNumber('5')">5</button>
        <button @click="appendNumber('6')">6</button>
        <button @click="add">+</button>
        <button @click="appendNumber('7')">7</button>
        <button @click="appendNumber('8')">8</button>
        <button @click="appendNumber('9')">9</button>
        <button @click="subtract">-</button>
        <button @click="addDecimalPoint">.</button>
        <button @click="appendNumber('0')">0</button>
        <button @click="equals" class="span-two">=</button>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      previousVal: null,
      currentVal: '',
      operation: null,
      operand: '',
      operatorClicked: false,
    }
  },
  methods: {
    clear() {
      this.currentVal = '';
      this.previousVal = null;
    },

    sign() {
      this.currentVal = this.currentVal.charAt(0) === '-' ? this.currentVal.slice(1) : `-${this.currentVal}`;
    },

    percent() {
      this.currentVal = `${parseFloat(this.currentVal / 100)}`;
    },

    appendNumber(number) {
      if (this.operatorClicked) {
        this.currentVal = '';
        this.operatorClicked = false;
      }
      this.currentVal = `${this.currentVal}${number}`;
    },

    addDecimalPoint() {
      if(this.currentVal.indexOf('.') === -1) {
        this.appendNumber('.');
      }
    },

    setPreviousVal() {
      this.previousVal = `${this.currentVal}${this.operand}`;
      // this.previousVal = this.currentVal;
      this.operatorClicked = true;
    },

    divide() {
      if (this.previousVal === null) {
        this.operation = (x, y) => x / y;
        this.operand = '÷';
        this.setPreviousVal();
      }
      else {
        this.currentVal = `${this.operation(parseFloat(this.currentVal), parseFloat(this.previousVal))}`;
        this.operand = '÷';
        this.setPreviousVal()
      }
      
    },

    multiply() {
      if (this.previousVal === null) {
        this.operation = (x, y) => x * y;
        this.operand = '*';
        this.setPreviousVal();
      }
      else {
        this.currentVal = `${this.operation(parseFloat(this.currentVal), parseFloat(this.previousVal))}`;
        this.operand= '*';
        this.setPreviousVal()
      }
    },

    add() {
      if (this.previousVal === null) {
        this.operation = (x, y) => x + y;
        this.operand = '+';
        this.setPreviousVal();
      }
      else {
        this.currentVal = `${this.operation(parseFloat(this.currentVal), parseFloat(this.previousVal))}`;
        this.operand = '+';
        this.setPreviousVal()
      }
    },

    subtract() {
      if (this.previousVal === null) { 
        this.operation = (x, y) => y - x;
        this.operand = '-';
        this.setPreviousVal();
      }
      else {
        this.currentVal = `${this.operation(parseFloat(this.currentVal), parseFloat(this.previousVal))}`;
        this.operand = '-';
        this.setPreviousVal()
        }
      },

    equals() {
      if(this.currentVal) {
        this.currentVal = `${this.operation(parseFloat(this.currentVal), parseFloat(this.previousVal))}`;
        this.previousVal = null;
      }
      
    }
  }
}
</script>

<style>
*, *::before, *::after{
    box-sizing: border-box;
    font-family: Arial, Helvetica, sans-serif;
    font-weight: normal;
}

body {
    padding: 0;
    margin: 0;
    background: linear-gradient(to right, rgb(204, 66, 193), rgb(104, 9, 53));
}

.calculator-grid {
    display: grid;
    justify-content: center;
    align-content: center;
    min-height: 100vh;
    grid-template-columns: repeat(4, 100px);
    grid-template-rows: minmax(120px, auto) repeat(5, 100px);
}

.calculator-grid > button {
    cursor: pointer;
    font-size: 2rem;
    border: 1px solid white;
    outline: none;
    background-color: rgba(255, 255, 255, .75);
}

.calculator-grid > button:hover {
    background-color: rgba(255, 255, 255, .9);
}

.span-two {
    grid-column: span 2;
}

.output {
    grid-column: 1 / -1;
    background-color: rgba(0, 0, 0, .75);
    display: flex;
    align-items: flex-end;
    justify-content: space-around;
    flex-direction: column;
    padding: 10px;
    word-wrap: break-word;
    word-break: break-all;
}

.output .previous-operand {
    color: rgba(255, 255, 255, .75);
    font-size: 1.5rem;
}

.output .current-operand {
    color: white;
    font-size: 2.5rem;
}
</style>
