<script setup>
import { ref } from 'vue';

const current = ref('');
const previous = ref(null);
const operator = ref(null);
const operatorClicked = ref(false);

/**
 * Reset everything
 */
function clearDisplay() {
  current.value = '';
  previous.value = null;
  operator.value = null;
  operatorClicked.value = false;
}

/**
 * Append a value to the display
 *
 * @param {string} value
 */
function append(value) {
  // Check to make sure display doesn't already have a decimal
  if (value === '.' && current.value.indexOf('.') !== -1) {
    return;
  }
  if (operatorClicked.value) {
    current.value = '';
    operatorClicked.value = false;
  }
  current.value = `${current.value}${value}`;
}

function setPrevious() {
  previous.value = current.value;
  operatorClicked.value = true;
}

function divide() {
  operator.value = (a, b) => a / b;
  setPrevious();
}

function multiply() {
  operator.value = (a, b) => a * b;
  setPrevious();
}

function add() {
  operator.value = (a, b) => a + b;
  setPrevious();
}

function subtract() {
  operator.value = (a, b) => a - b;
  setPrevious();
}

function calculate() {
  current.value = `${operator.value(
    parseFloat(previous.value),
    parseFloat(current.value)
  )}`;
  previous.value = null;
}


</script>

<template>
  <div class="calculator">
    <div class="display">{{ current || '0' }}</div>
    <span @click="clearDisplay" class="button action">C</span>
    <span class="button action">MC</span>
    <span class="button action">M+</span>
    <span class="button action">MR</span>
    <span @click="append('7')" class="button">7</span>
    <span @click="append('8')" class="button">8</span>
    <span @click="append('9')" class="button">9</span>
    <span @click="add()" class="button operator">+</span>
    <span @click="append('4')" class="button">4</span>
    <span @click="append('5')" class="button">5</span>
    <span @click="append('6')" class="button">6</span>
    <span @click="subtract()" class="button operator">-</span>
    <span @click="append('1')" class="button">1</span>
    <span @click="append('2')" class="button">2</span>
    <span @click="append('3')" class="button">3</span>
    <span @click="multiply" class="button operator">&times;</span>
    <span @click="append('0')" class="button">0</span>
    <span @click="append('.')" class="button">.</span>
    <span @click="calculate()" class="button action">=</span>
    <span @click="divide()" class="button operator">/</span>
  </div>
</template>

<style scoped>
  .calculator {
    width: 50%;
    font-size: 20px;
    margin: auto;
    display: grid;
    border-radius: 2em;
    grid-template-columns: repeat(4, 1fr);
    grid-auto-rows: minmax(50px, auto);
  }
  .display {
    grid-column: 1 / 5;
    color: #ffffff;
    background-color: #333;
    display: grid;
    align-items: center;
    justify-items: right;
    padding: 0 1em;
  }
  .button {
    text-align: center;
    background-color: #ffffff;
    border: 1px solid #c5c5c5;
    display: grid;
    align-items: center;
    justify-items: center;
  }
  .operator {
    background-color: #e5e7ec;
  }
  .action {
    background-color: #fa9209;
    border: 1px solid #ca7a11;
  }
</style>
