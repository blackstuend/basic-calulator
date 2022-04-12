<script setup lang="ts">
import { ref } from "vue";

const inputValue = ref<string>("");
const inputRef = ref<HTMLElement | null>(null);
interface calculatorButton {
  text: string;
  clickMethod(): void;
}

const numberButtons = ref<calculatorButton[]>([
  {
    text: "(",
    clickMethod: () => {
      inputValue.value += "( ";

      inputRef.value?.focus();
    },
  },
  {
    text: ")",
    clickMethod: () => {
      inputValue.value += " )";

      inputRef.value?.focus();
    },
  },
  {
    text: "%",
    clickMethod: () => {
      inputValue.value += "%";

      inputRef.value?.focus();
    },
  },
  {
    text: "AC",
    clickMethod: () => {
      inputValue.value = "";

      inputRef.value?.focus();
    },
  },
  {
    text: "7",
    clickMethod: () => {
      inputValue.value += "7";

      inputRef.value?.focus();
    },
  },
  {
    text: "8",
    clickMethod: () => {
      inputValue.value += "8";

      inputRef.value?.focus();
    },
  },
  {
    text: "9",
    clickMethod: () => {
      inputValue.value += "9";

      inputRef.value?.focus();
    },
  },
  {
    text: "/",
    clickMethod: () => {
      inputValue.value += " / ";

      inputRef.value?.focus();
    },
  },
  {
    text: "4",
    clickMethod: () => {
      inputValue.value += "4";

      inputRef.value?.focus();
    },
  },
  {
    text: "5",
    clickMethod: () => {
      inputValue.value += "5";

      inputRef.value?.focus();
    },
  },
  {
    text: "6",
    clickMethod: () => {
      inputValue.value += "6";

      inputRef.value?.focus();
    },
  },
  {
    text: "x",
    clickMethod: () => {
      inputValue.value += " x ";

      inputRef.value?.focus();
    },
  },
  {
    text: "1",
    clickMethod: () => {
      inputValue.value += "1";

      inputRef.value?.focus();
    },
  },
  {
    text: "2",
    clickMethod: () => {
      inputValue.value += "2";

      inputRef.value?.focus();
    },
  },
  {
    text: "3",
    clickMethod: () => {
      inputValue.value += "3";

      inputRef.value?.focus();
    },
  },
  {
    text: "-",
    clickMethod: () => {
      inputValue.value += " - ";

      inputRef.value?.focus();
    },
  },
  {
    text: "0",
    clickMethod: () => {
      inputValue.value += "0";

      inputRef.value?.focus();
    },
  },
  {
    text: ".",
    clickMethod: () => {
      inputValue.value += ".";

      inputRef.value?.focus();
    },
  },
  {
    text: "=",
    clickMethod: () => {
      startCalculate();
    },
  },
  {
    text: "+",
    clickMethod: () => {
      inputValue.value += " + ";

      inputRef.value?.focus();
    },
  },
]);

// calculate inputRef's value
function startCalculate(): void {
  // trim space
  let calculateString = inputValue.value.replace(/\s/g, "");

  // replace x to *
  calculateString = calculateString.replace(/x/g, "*");

  let result = calculate(calculateString);

  // calculator failed, show alert
  if (isNaN(result)) {
    alert("format is error");

    inputValue.value = "";
    return;
  }

  inputValue.value = result.toString();

  inputRef.value?.focus();
}

// basic operator
// e.x: 1 + 5 * 8 = 41
function basicOperator(value: string): number {
  const reg = /(\d+\.*\d*|\+|\/|\*|-|%)/g;
  const arr = value.match(reg);

  if (!arr) {
    return NaN;
  } else if (arr.length === 1) {
    return Number(arr[0]);
  }

  try {
    // handle advance operator
    if (arr?.length) {
      for (let i = 0; i < arr.length; i++) {
        if (arr[i] === "*") {
          let result = Number(arr[i - 1]) * Number(arr[i + 1]);
          arr.splice(i - 1, 3, result.toString());
          i--;
        } else if (arr[i] === "/") {
          let result = Number(arr[i - 1]) / Number(arr[i + 1]);
          arr.splice(i - 1, 3, result.toString());
          i--;
        } else if (arr[i] === "%") {
          let result = Number(arr[i - 1]) % Number(arr[i + 1]);
          arr.splice(i - 1, 3, result.toString());
          i--;
        }
      }
    }

    // handle basic operator
    if (arr?.length) {
      for (let i = 0; i < arr.length; i++) {
        if (arr[i] === "+") {
          let result = Number(arr[i - 1]) + Number(arr[i + 1]);
          arr.splice(i - 1, 3, result.toString());
          i--;
        } else if (arr[i] === "-") {
          let result = Number(arr[i - 1]) - Number(arr[i + 1]);
          arr.splice(i - 1, 3, result.toString());
          i--;
        }
      }
    }

    return Number(arr[0]);
  } catch (e) {
    return NaN;
  }
}

// calculate
function calculate(value: string): number {
  try {
    // handle bucket
    let bucketsCount = 0;
    let inBucketsText = "";
    let basicValue = "";

    for (let text of value) {
      if (text === "(") {
        if (bucketsCount > 0) {
          inBucketsText += text;
        }

        bucketsCount++;
      } else if (text === ")") {
        if (bucketsCount <= 0) {
          throw new Error("buckets not match");
        } else {
          bucketsCount--;

          if (bucketsCount > 0) {
            inBucketsText += text;
          } else if (bucketsCount === 0) {
            basicValue += calculate(inBucketsText);
          }
        }
      } else if (bucketsCount > 0) {
        inBucketsText += text;
      } else {
        basicValue += text;
      }
    }

    return basicOperator(basicValue);
  } catch (e) {
    return NaN;
  }
}
</script>

<template>
  <div class="calculator">
    <input
      type="text"
      class="calculator-input"
      ref="inputRef"
      v-model="inputValue"
      @keyup.enter="startCalculate"
    />
    <ul class="calculator-buttonList">
      <li
        class="calculator-buttonList-button"
        v-for="(item, key) in numberButtons"
        :key="key"
        @click="item.clickMethod"
      >
        {{ item.text }}
      </li>
    </ul>
  </div>
</template>

<style scoped>
* {
  margin: 0;
  box-sizing: border-box;
  padding: 0;
}

.calculator {
  margin: 0 auto;
  border: 1px solid #ccc;
  padding: 1rem;
  border-radius: 5px;
  max-width: 500px;
  margin: 30px auto;
}

.calculator-input {
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 0.875rem;
  margin-bottom: 2rem;
  width: 100%;
  font-size: 1.25rem;
}

.calculator-input:focus {
  border-color: #66afe9;
  box-shadow: inset 0 1px 1px rgb(0 0 0 / 8%), 0 0 8px rgb(102 175 233 / 60%);
  outline: none;
}

.calculator-buttonList {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  margin: 0 auto;
  gap: 0.875rem;
}

.calculator-buttonList-button {
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 0.875rem 1rem;
  list-style: none;
  text-align: center;
  font-weight: 700;
}

.calculator-buttonList-button:hover {
  background-color: #d6d6d6;
}

.calculator-buttonList-button:active {
  background-color: #d6d6d6;
}
</style>
