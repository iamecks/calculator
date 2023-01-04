<template>
  <div class="container col-2 bg-black text-white mt-5 mx-auto px-1 rounded-1">
    <!-- Result/Input Display -->
    <div class="row gx-1 mt-1">
      <h1 class="text-white text-end">{{ state.displayValue }}</h1>
    </div>

    <!-- Row #1 -->
    <div class="row gx-1 mt-1">
      <div class="col" @click="resetCurrentDisplayValue">
        <div class="h6 p-3 mb-0 bg-dark text-center rounded-1">CE</div>
      </div>
      <div class="col" @click="resetAll">
        <div class="h6 p-3 mb-0 bg-dark text-center rounded-1">AC</div>
      </div>
      <div class="col" @click="deleteLatestDigit">
        <div class="h6 p-3 mb-0 bg-dark text-center rounded-1">DEL</div>
      </div>
      <div class="col" @click="performOperation('/')">
        <div class="h6 p-3 mb-0 bg-dark text-center rounded-1">÷</div>
      </div>
    </div>

    <!-- Row #2 -->
    <div class="row gx-1 mt-1">
      <div class="col" @click="updateDisplay('7')">
        <div class="h6 p-3 mb-0 bg-secondary text-center rounded-1">7</div>
      </div>
      <div class="col" @click="updateDisplay('8')">
        <div class="h6 p-3 mb-0 bg-secondary text-center rounded-1">8</div>
      </div>
      <div class="col" @click="updateDisplay('9')">
        <div class="h6 p-3 mb-0 bg-secondary text-center rounded-1">9</div>
      </div>
      <div class="col" @click="performOperation('x')">
        <div class="h6 p-3 mb-0 bg-dark text-center rounded-1">x</div>
      </div>
    </div>

    <!-- Row #3 -->
    <div class="row gx-1 mt-1">
      <div class="col" @click="updateDisplay('4')">
        <div class="h6 p-3 mb-0 bg-secondary text-center rounded-1">4</div>
      </div>
      <div class="col" @click="updateDisplay('5')">
        <div class="h6 p-3 mb-0 bg-secondary text-center rounded-1">5</div>
      </div>
      <div class="col" @click="updateDisplay('6')">
        <div class="h6 p-3 mb-0 bg-secondary text-center rounded-1">6</div>
      </div>
      <div class="col" @click="performOperation('-')">
        <div class="h6 p-3 mb-0 bg-dark text-center rounded-1">-</div>
      </div>
    </div>

    <!-- Row #4 -->
    <div class="row gx-1 mt-1">
      <div class="col" @click="updateDisplay('1')">
        <div class="h6 p-3 mb-0 bg-secondary text-center rounded-1">1</div>
      </div>
      <div class="col" @click="updateDisplay('2')">
        <div class="h6 p-3 mb-0 bg-secondary text-center rounded-1">2</div>
      </div>
      <div class="col" @click="updateDisplay('3')">
        <div class="h6 p-3 mb-0 bg-secondary text-center rounded-1">3</div>
      </div>
      <div class="col" @click="performOperation('+')">
        <div class="h6 p-3 mb-0 bg-dark text-center rounded-1">+</div>
      </div>
    </div>

    <!-- Row #5 -->
    <div class="row gx-1 mt-1">
      <div class="col mb-1" @click="updateDisplay('!')">
        <div class="h6 p-3 mb-0 bg-secondary text-center rounded-1">+/-</div>
      </div>
      <div class="col" @click="updateDisplay('0')">
        <div class="h6 p-3 mb-0 bg-secondary text-center rounded-1">0</div>
      </div>
      <div class="col" @click="updateDisplay('.')">
        <div class="h6 p-3 mb-0 bg-secondary text-center rounded-1">.</div>
      </div>
      <div class="col" @click="performOperation('=')">
        <div class="h6 p-3 mb-0 bg-primary text-center rounded-1">=</div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive } from "vue";

const INITIAL_STATE = {
  displayValue: "0",
  firstOperand: null,
  waitingForSecondOperand: false,
  operator: null,
};

let state = reactive({ ...INITIAL_STATE });

function updateDisplay(value) {
  if (state.waitingForSecondOperand) {
    state.displayValue = value;
    state.waitingForSecondOperand = false;
  } else {
    state.displayValue =
      state.displayValue === "0" ? value : state.displayValue + value;
  }
}

function performOperation(newOperator) {
  if (state.firstOperand == null) {
    state.firstOperand = parseFloat(state.displayValue);
    state.waitingForSecondOperand = true;
    state.operator = newOperator;
  } else if (newOperator === "=") {
    const result = eval(
      `${state.firstOperand} ${state.operator} ${state.displayValue}`
    );
    resetAll();
    state.displayValue = String(result);
  } else if (state.operator) {
    // Check previous operator !=
    if (state.operator === "=") {
      alert("Invalid Operand!");
      resetAll();
      return;
    }

    const result = eval(
      `${state.firstOperand} ${state.operator} ${state.displayValue}`
    );
    state.displayValue = String(result);
    state.firstOperand = result;
    state.waitingForSecondOperand = true;
    state.operator = newOperator;
  }
}

function resetAll() {
  state.displayValue = INITIAL_STATE.displayValue;
  state.firstOperand = INITIAL_STATE.firstOperand;
  state.waitingForSecondOperand = INITIAL_STATE.waitingForSecondOperand;
  state.operator = INITIAL_STATE.operator;
}

function resetCurrentDisplayValue() {
  state.displayValue = "0";
}

function deleteLatestDigit() {
  const displayValue = state.displayValue;

  // Trim off last digit
  state.displayValue = displayValue.substring(0, displayValue.length - 1);

  // If empty set to the default placeholder value
  if (state.displayValue === "") state.displayValue = "0";
}
</script>
