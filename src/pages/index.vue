
<script lang="ts" setup>
import { ref } from 'vue'

const current = ref('')
const previous = ref('')
const operator = ref<Function>(() => undefined)
const operatorClicked = ref(false)
const equalVal = ref('')

const handleClear = () => {
  current.value = ''
  previous.value = ''
  equalVal.value = ''
  operatorClicked.value = false
  operator.value = () => undefined
}

const handleSign = () => {
  if (current.value.charAt(0) === '-') {
    current.value = current.value.slice(1)
  }
  else {
    current.value = current.value.charAt(0) === '-'
      ? current.value.slice(1)
      : `-${current.value}`
  }
}

const handlePercent = () => {
  current.value = `${parseFloat(current.value) / 100}`
}

const handleAppend = (value: string) => {
  if (operatorClicked.value) {
    current.value = value
    operatorClicked.value = false
  }
  else {
    current.value += value
  }
}

const handleEqual = () => {
  if (!equalVal.value)
    equalVal.value = current.value

  if (previous.value) {
    current.value = `${operator.value(parseFloat(previous.value), parseFloat(current.value))}`
    previous.value = ''
  }
  else {
    current.value = `${operator.value(parseFloat(equalVal.value), parseFloat(current.value))}`
  }
}

const handleDot = () => {
  if (!current.value.includes('.'))
    current.value += '.'
}

const setPrevious = () => {
  operatorClicked.value = true

  if (previous.value && current.value)
    handleEqual()

  previous.value = current.value
}

const handleDivide = () => {
  equalVal.value = ''
  setPrevious()
  operator.value = (a: number, b: number) => a / b
}

const handleTimes = () => {
  equalVal.value = ''
  setPrevious()
  operator.value = (a: number, b: number) => a * b
}

const handleMinus = () => {
  equalVal.value = ''
  setPrevious()
  operator.value = (a: number, b: number) => a - b
}

const handleAdd = () => {
  equalVal.value = ''
  setPrevious()
  operator.value = (a: number, b: number) => a + b
}

</script>

<template>
  <div class="m-0 m-auto max-w-356px grid text-40px grid-cols-[repeat(4,1fr)]  calculator">
    <div class="display overflow-hidden px-4 text-right">
      {{ current || '0' }}
    </div>
    <div class="btn" @click="handleClear">
      C
    </div>
    <div class="btn" @click="handleSign">
      +/-
    </div>
    <div class="btn" @click="handlePercent">
      %
    </div>
    <div class="btn operator" @click="handleDivide">
      ÷
    </div>
    <div class="btn" @click="handleAppend('7')">
      7
    </div>
    <div class="btn" @click="handleAppend('8')">
      8
    </div>
    <div class="btn" @click="handleAppend('9')">
      9
    </div>
    <div class="btn operator" @click="handleTimes">
      x
    </div>
    <div class="btn" @click="handleAppend('4')">
      4
    </div>
    <div class="btn" @click="handleAppend('5')">
      5
    </div>
    <div class="btn" @click="handleAppend('6')">
      6
    </div>
    <div class="btn operator" @click="handleMinus">
      -
    </div>
    <div class="btn" @click="handleAppend('1')">
      1
    </div>
    <div class="btn" @click="handleAppend('2')">
      2
    </div>
    <div class="btn" @click="handleAppend('3')">
      3
    </div>
    <div class="btn operator" @click="handleAdd">
      +
    </div>
    <div class="btn zero" @click="handleAppend('0')">
      0
    </div>
    <div class="btn" @click="handleDot">
      .
    </div>
    <div class="btn operator" @click="handleEqual">
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
  display: flex;
  align-items: center;
  justify-content: center;
}

.operator {
  background-color: rgb(242,162,60);
  color: white;
}
</style>
