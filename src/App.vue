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
    </div>

    <!-- 添加新的科目 -->
    <div class="addItem">
      <input type="text" name="score" v-model="score" />
      <input type="text" name="credit" v-model="credit" />
      <div @click="add" class="button">确定</div>
      <div @click="clear" class="button">清除全部</div>
    </div>

    <!-- 计算综合 -->
    <div class=""></div>
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

const arr: suject[] = reactive([])
const add = () => {
  const item: suject = { score: score.value, credit: credit.value }
  arr.push(item)
  score.value = 0
  credit.value = 0
}

const clear = () => {
  arr.length = 0
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
  color: #ffe3e3;
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
}
</style>
