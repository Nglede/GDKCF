<template>
  <div class="app">
    <h1>规定课程分</h1>
    <!-- 展示已经添加的科目 -->
    <div class="table">
      <div class="header">
        <div class="header-item">成绩</div>
        <div class="header-item">学分</div>
      </div>

      <div class="main" v-for="(item,index) in arr" :key="index">
        <div class="main-item">{{ item.score }}</div>
        <div class="main-item">{{ item.credit }}</div>
      </div>

      <div class="header" v-if="electiveArr.length !== 0">
        <div class="header-item">选修</div>
      </div>

      <div class="main" v-for="(item,index) in electiveArr" :key="index">
        <div class="main-item">{{ item.score }}</div>
        <div class="main-item">{{ item.credit }}</div>
      </div>
    </div>

    <!-- 添加新的科目 -->
    <div class="addItem">
      <input type="text" name="score" v-model="score" placeholder="成绩" />
      <input type="text" name="credit" v-model="credit" placeholder="学分" />
      <div @click="add()" class="button">确定</div>
      <div @click="elective = !elective" class="button">切换到{{ elective ? '必修' : '选修' }}</div>
      <div @click="clear" class="button">清除全部</div>
    </div>

    <!-- 计算综合 -->
    <div class="sum">
      <div class="submit" @click="submit">计算课程分</div>
      <div class="finsh"> {{ returnVal }} </div>
    </div>
  </div>
</template>

<script setup lang="ts">
interface suject {
  score: number
  credit: number
}

import { ref, reactive } from 'vue'
let score = ref()
let credit = ref()
let returnVal = ref(0)

const elective = ref(false)
const arr: suject[] = reactive([])
const electiveArr: suject[] = reactive([])
const add = () => {
  if (!score.value && !credit.value) return
  const item: suject = { score: score.value, credit: credit.value }
  if (elective.value) {
    electiveArr.push(item)
  } else {
    arr.push(item)
  }
  score.value = ''
  credit.value = ''
}

const clear = () => {
  arr.length = 0
  electiveArr.length = 0
  returnVal.value = 0
}

const submit = () => {
  if (arr.length === 0) return

  let bxScore = 0
  let bxCredit = 0
  let xxScore = 0
  let xxCredit = 0
  let res = 0
  arr.forEach(item => {
    bxScore += item.score * item.credit
    bxCredit += item.credit
  })

  if (electiveArr.length === 0) {
    res = bxScore / bxCredit
  } else {
    electiveArr.forEach(item => {
      xxScore += item.score * item.credit
      xxCredit += item.credit
    })

    res = ((bxScore / bxCredit) * 100 * 0.85 / 100) + ((xxScore / bxCredit) * 100 * 0.15 / 100)
  }
  returnVal.value = res
}

</script>

<style>
body,
html {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  background-color: #93b5c6;
}

.app {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

input {
  background: none;
  outline: none;
  border: 0px;
}

h1 {
  color: #fff;
}

.table {
  background: #fff;
  width: 50vw;
  min-height: 30vh;
  border-radius: 20px;
}

.table .header,
.table .main {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  padding: 0.5em;
  text-align: center;
}

.addItem {
  width: 59vw;
  height: 100%;
  margin-top: 60px;
  display: flex;
  justify-content: space-around;
}

.addItem input {
  background: #fff;
  outline: none;
  min-width: 50px;
  margin-right: 10px;
  border-radius: 5px;
  height: 30px;
  line-height: 30px;
  text-align: center;
  font-weight: 600;
}

.button {
  background: #ffe3e3;
  border-radius: 5px;
  width: 100px;
  height: 30px;
  line-height: 30px;
  text-align: center;
  color: #4596b3;
  font-weight: 600;
  cursor: pointer;
}

.sum {
  background: #ffe3e3;
  border-radius: 5px;
  margin-top: 30px;
  width: 200px;
  height: 50px;
  line-height: 50px;
  text-align: center;
  color: #4596b3;
  font-weight: 600;
  cursor: pointer;
}

.finsh {
  margin-top: 60px;
  width: 200px;
  height: 200px;
  font-size: 50px;
  color: #fff;
}
</style>
