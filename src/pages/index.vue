
<script lang="ts" setup>
import { ref } from 'vue'

const current = ref('')
const previous = ref('')
const operator = ref<Function>(() => undefined)
const operatorClicked = ref(false)

const clear = () => {
  current.value = ''
  previous.value = ''
  operatorClicked.value = false
  operator.value = () => undefined
}

const sign = () => {
  if (current.value.charAt(0) === '-') {
    current.value = current.value.slice(1)
  }
  else {
    current.value = current.value.charAt(0) === '-'
      ? current.value.slice(1)
      : `-${current.value}`
  }
}

const percent = () => {
  current.value = `${parseFloat(current.value) / 100}`
}

const append = (value: string) => {
  if (operatorClicked.value) {
    current.value = value
    operatorClicked.value = false
  }
  else {
    current.value += value
  }
}

const equal = () => {
  current.value = `${operator.value(parseFloat(previous.value), parseFloat(current.value))}`
  previous.value = ''
}

const dot = () => {
  if (!current.value.includes('.'))
    current.value += '.'
}

const setPrevious = () => {
  operatorClicked.value = true

  if (previous.value && current.value)
    equal()

  previous.value = current.value
}

const divide = () => {
  setPrevious()
  operator.value = (a: number, b: number) => a / b
}

const times = () => {
  setPrevious()
  operator.value = (a: number, b: number) => a * b
}

const minus = () => {
  setPrevious()
  operator.value = (a: number, b: number) => a - b
}

const add = () => {
  setPrevious()
  operator.value = (a: number, b: number) => a + b
}

</script>

<template>
  <div class="m-0 m-auto max-w-360px grid text-40px grid-cols-[repeat(4,1fr)]  calculator">
    <div class="display overflow-hidden px-2">
      {{ current || '0' }}
    </div>
    <div class="btn" @click="clear">
      C
    </div>
    <div class="btn" @click="sign">
      +/-
    </div>
    <div class="btn" @click="percent">
      %
    </div>
    <div class="btn operator" @click="divide">
      ÷
    </div>
    <div class="btn" @click="append('7')">
      7
    </div>
    <div class="btn" @click="append('8')">
      8
    </div>
    <div class="btn" @click="append('9')">
      9
    </div>
    <div class="btn operator" @click="times">
      x
    </div>
    <div class="btn" @click="append('4')">
      4
    </div>
    <div class="btn" @click="append('5')">
      5
    </div>
    <div class="btn" @click="append('6')">
      6
    </div>
    <div class="btn operator" @click="minus">
      -
    </div>
    <div class="btn" @click="append('1')">
      1
    </div>
    <div class="btn" @click="append('2')">
      2
    </div>
    <div class="btn" @click="append('3')">
      3
    </div>
    <div class="btn operator" @click="add">
      +
    </div>
    <div class="btn zero" @click="append('0')">
      0
    </div>
    <div class="btn" @click="dot">
      .
    </div>
    <div class="btn operator" @click="equal">
      =
    </div>
  </div>
</template>

<style scoped>
.calculator {
  grid-auto-rows: minmax(50px, auto);
}

.display {
  grid-column: 1 / 5;
  background-color: rgb(58,53,53);
  color: white;
  text-overflow: ellipsis;
}

.zero {
  grid-column: 1 / 3;
}

.btn {
  background-color: rgb(100,100,100);
  border: 1px solid rgb(49,49,49);
}

.operator {
  background-color: rgb(242,162,60);
  color: white;
}
</style>
