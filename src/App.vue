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
      calculate(inputValue.value);

      inputRef.value?.focus();
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

function calculate(value: string) {
  // trim value
  value = value.replace(/\s/g, "");

  // replace x to js's Multiplication *
  value = value.replace(/x/g, "*");

  inputValue.value = eval(value);
}
</script>

<template>
  <div class="calculator">
    <input
      type="text"
      class="calculator-input"
      ref="inputRef"
      v-model="inputValue"
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
  gap: 1rem;
}

.calculator-buttonList-button {
  border: 1px solid #ccc;
  border-radius: 5px;
  padding: 1rem 2rem;
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
