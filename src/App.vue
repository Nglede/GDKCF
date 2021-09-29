<template>
  <div class="app">
    <h1>规定课程分</h1>
    <!-- 展示已经添加的科目 -->
    <div class="table"></div>

    <!-- 添加新的科目 -->
    <div class="addItem">
      <input type="text" name="score" v-model="score" />
      <input type="text" name="credit" v-model="credit" />
      <div @click='add'>add</div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
let score = ref(0)
let credit = ref(0)

const arr: Array<Object> = []
const add = () => {
  const item: Object = { score, credit }
  arr.push(item)
  score.value = 0
  credit.value = 0
}

</script>

<style>
.app {
  text-align: center;
  margin-top: 60px;
}

.addItem {
  display: flex;
}
</style>
