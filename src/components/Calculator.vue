<template>
  <div class="container">
    <form>
      <div class="row gx-1 mt-5">
        <div class="form-group">
          <label for="result">Result</label>
          <input
            v-if="operator && secondGivenNumber"
            type="number"
            class="form-control"
            id="result"
            v-model="secondGivenNumber"
          />

          <input
            v-else
            type="number"
            class="form-control"
            id="result"
            v-model="firstGivenNumber"
          />
        </div>
      </div>

      <div class="row gx-1 mt-1">
        <div class="col" @click="clearAll">
          <div class="p-3 border bg-light">CE</div>
        </div>
        <div class="col" @click="clearAll">
          <div class="p-3 border bg-light">C</div>
        </div>
        <div class="col" @click="clearAll">
          <div class="p-3 border bg-light">DEL</div>
        </div>
        <div class="col" @click="selectOperator('/')">
          <div class="p-3 border bg-light">/</div>
        </div>
      </div>
      <div class="row gx-1 mt-1">
        <div class="col" @click="concatInputVal('7')">
          <div class="p-3 border bg-light">7</div>
        </div>
        <div class="col" @click="concatInputVal('8')">
          <div class="p-3 border bg-light">8</div>
        </div>
        <div class="col" @click="concatInputVal('9')">
          <div class="p-3 border bg-light">9</div>
        </div>
        <div class="col" @click="selectOperator('x')">
          <div class="p-3 border bg-light">x</div>
        </div>
      </div>
      <div class="row gx-1 mt-1">
        <div class="col" @click="concatInputVal('4')">
          <div class="p-3 border bg-light">4</div>
        </div>
        <div class="col" @click="concatInputVal('5')">
          <div class="p-3 border bg-light">5</div>
        </div>
        <div class="col" @click="concatInputVal('6')">
          <div class="p-3 border bg-light">6</div>
        </div>
        <div class="col" @click="selectOperator('-')">
          <div class="p-3 border bg-light">-</div>
        </div>
      </div>
      <div class="row gx-1 mt-1">
        <div class="col" @click="concatInputVal('1')">
          <div class="p-3 border bg-light">1</div>
        </div>
        <div class="col" @click="concatInputVal('2')">
          <div class="p-3 border bg-light">2</div>
        </div>
        <div class="col" @click="concatInputVal('3')">
          <div class="p-3 border bg-light">3</div>
        </div>
        <div class="col" @click="selectOperator('+')">
          <div class="p-3 border bg-light">+</div>
        </div>
      </div>
      <div class="row gx-1 mt-1">
        <div class="col" @click="concatInputVal('n')">
          <div class="p-3 border bg-light">+/-</div>
        </div>
        <div class="col" @click="concatInputVal('0')">
          <div class="p-3 border bg-light">0</div>
        </div>
        <div class="col" @click="concatInputVal('.')">
          <div class="p-3 border bg-light">.</div>
        </div>
        <div class="col" @click="checkResult">
          <div class="p-3 border bg-light">=</div>
        </div>
      </div>
    </form>
  </div>
</template>

<script setup>
import { ref } from "vue";

const firstGivenNumber = ref("");
const secondGivenNumber = ref("0");
const operator = ref("");

function clearAll() {
  firstGivenNumber.value = "";
  secondGivenNumber.value = "0";
  operator.value = "";
}

function concatInputVal(inputVal) {
  // Clear placeholder "0"
  if (secondGivenNumber.value === "0") secondGivenNumber.value = "";

  // If operator already concat on latest number
  if (operator.value)
    secondGivenNumber.value = secondGivenNumber.value.concat(inputVal);
  // Default: only on 1st number
  else firstGivenNumber.value = firstGivenNumber.value.concat(inputVal);
}

function selectOperator(inputOperator) {
  if (secondGivenNumber.value) firstGivenNumber.value = secondGivenNumber.value;
  operator.value = inputOperator;
}

function checkResult() {
  const parsed1stNum = parseInt(firstGivenNumber.value);
  const parsed2ndNum = parseInt(secondGivenNumber.value);
  const key = operator.value;

  switch (key) {
    case "/":
      secondGivenNumber.value = parsed1stNum / parsed2ndNum;
      break;

    case "x":
      secondGivenNumber.value = parsed1stNum * parsed2ndNum;
      break;

    case "-":
      secondGivenNumber.value = parsed1stNum - parsed2ndNum;
      break;

    case "+":
      secondGivenNumber.value = parsed1stNum + parsed2ndNum;
      break;

    default:
      alert("Something Wrong");
      break;
  }
}
</script>
