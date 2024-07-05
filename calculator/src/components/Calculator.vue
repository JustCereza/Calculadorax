<template>
  <div class="calculator-container">
    <div class="calculator">
      <h1>{{ msg }}</h1>
      <table class="table table-bordered">
        <tbody>
          <tr class="output">
            <td colspan="4" class="output-field">
              {{ output || 0 }}
            </td>
          </tr>
          <tr>
            <td v-on:click="clearField">C</td>
            <td v-on:click="setNegativeOrPositive">+/-</td>
            <td v-on:click="calculatePorcent">%</td>
            <td class="lastcolumn" v-on:click="setOperation('/')">÷</td>
          </tr>
          <tr>
            <td v-on:click="getNumber('7')">7</td>
            <td v-on:click="getNumber('8')">8</td>
            <td v-on:click="getNumber('9')">9</td>
            <td class="lastcolumn" v-on:click="setOperation('*')">x</td>
          </tr>
          <tr>
            <td v-on:click="getNumber('4')">4</td>
            <td v-on:click="getNumber('5')">5</td>
            <td v-on:click="getNumber('6')">6</td>
            <td class="lastcolumn" v-on:click="setOperation('-')">-</td>
          </tr>
          <tr>
            <td v-on:click="getNumber('1')">1</td>
            <td v-on:click="getNumber('2')">2</td>
            <td v-on:click="getNumber('3')">3</td>
            <td class="lastcolumn" v-on:click="setOperation('+')">+</td>
          </tr>
          <tr>
            <td colspan="2" v-on:click="getNumber('0')">0</td>
            <td v-on:click="getNumber('.')">.</td>
            <td class="lastcolumn" v-on:click="calculateResult">=</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      output: '',
      currentOperation: null,
      previousValue: null
    }
  },
  methods: {
    clearField() {
      this.output = '';
      this.currentOperation = null;
      this.previousValue = null;
    },
    setNegativeOrPositive() {
      if (this.output) {
        this.output = this.output[0] === '-' ? this.output.slice(1) : `-${this.output}`;
      }
    },
    calculatePorcent() {
      if (this.output) {
        this.output = (parseFloat(this.output) / 100).toString();
      }
    },
    getNumber(number) {
      if (this.output === '0' && number === '0') return;
      this.output = `${this.output}${number}`;
    },
    setOperation(operation) {
      if (this.output) {
        this.currentOperation = operation;
        this.previousValue = this.output;
        this.output = '';
      }
    },
    calculateResult() {
      if (this.output && this.currentOperation && this.previousValue !== null) {
        const currentValue = parseFloat(this.output);
        const previousValue = parseFloat(this.previousValue);

        let result;
        switch (this.currentOperation) {
          case '+':
            result = previousValue + currentValue;
            break;
          case '-':
            result = previousValue - currentValue;
            break;
          case '*':
            result = previousValue * currentValue;
            break;
          case '/':
            result = previousValue / currentValue;
            break;
        }

        this.output = result.toString();
        this.currentOperation = null;
        this.previousValue = null;
      }
    }
  }
}
</script>

<style scoped>
.calculator-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.calculator {
  width: 300px;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 10px;
  background-color: #f9f9f9;
}

h1 {
  text-align: center;
  margin-bottom: 20px;
}

.table {
  width: 100%;
}

.output-field {
  background-color: #000;
  color: #fff;
  font-size: 24px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  text-align: right;
  padding-right: 10px;
}

.table td {
  width: 25%;
  height: 60px;
  text-align: center;
  vertical-align: middle;
  font-size: 18px;
  cursor: pointer;
}

.lastcolumn {
  background-color: orange;
  color: #fff;
}

.lastcolumn:active {
  background-color: #000;
  color: #fff;
}
</style>
