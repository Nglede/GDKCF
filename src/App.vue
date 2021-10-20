<template>
  <div class="app">
    <h1>课程分</h1>
    <div class="content">
      <div class="header">
        <div class="select">
          <select v-model="courseSelected">
            <option :value="item" v-for="item in courseList">{{ item }}</option>
          </select>
          <select v-model="creditSelected">
            <option :value="item" v-for="item in creditList">{{ item }}</option>
          </select>
        </div>
        <div class="button" @click="submit">添 加</div>
      </div>
      <input type="text" name="input" v-model="score" placeholder="分数以空格间隔 如 90 90 89" />
      <div class="record">
        <!-- <div class="button" @click="calReslust">计 算</div> -->
        <div v-if="bxArr || xxArr" class="button" @click="clear">清 空</div>
        <!-- <div class="button" @click="calReslust">查 看</div> -->
      </div>
      <div class="reslust">
        <div class="text">{{ res }}</div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, Ref } from 'vue';

interface suject {
  score: number,
  credit: number
}

const courseList = ['必修', '选修']
const creditList = [1, 1.5, 2, 2.5, 3, 3.5, 4, 4.5]

const score = ref('')
const courseSelected = ref('必修')
const creditSelected = ref('1')
const bxArr = ref<suject[]>([])
const xxArr = ref<suject[]>([])

const bxScore = ref<number>(0)
const xxScore = ref<number>(0)
const bxCredit = ref<number>(0)
const xxCredit = ref<number>(0)
const res = ref<number>(0)

// 判断 必修/选修
const addSuject = (courseArr: Ref<suject[]>, arr: suject[]): void => {
  if (courseArr.value === void 0) {
    courseArr.value = [...arr]
  } else {
    courseArr.value = courseArr.value.concat(arr)
  }
}

// 计算 成绩 * 学分 之和
const calSujectSum = (arr: suject[]): number => {
  let res = 0
  arr.forEach(item => {
    res += item.score * item.credit
  })
  return res
}

// 计算学分和
const calCreditSum = (arr: suject[]): number => {
  let res = 0
  arr.forEach(item => {
    res += item.credit
  })
  return res
}

// 计算课程分
const calReslust = ():number => {
  console.log(xxArr.value.length);
  
  let res = 0
  bxScore.value = calSujectSum(bxArr.value)
  bxCredit.value = calCreditSum(bxArr.value)
  if (xxArr.value.length) {
    xxScore.value = calSujectSum(xxArr.value)
    xxCredit.value = calCreditSum(xxArr.value)

    let bxTemp = (bxScore.value / bxCredit.value)
    let xxTemp = (xxScore.value / xxCredit.value)
    bxTemp = Math.floor(bxTemp * 100) / 100
    xxTemp = Math.floor(xxTemp * 100) / 100

    res = bxTemp * 0.85 + xxTemp * 0.15
  } else {
    let bxTemp = (bxScore.value / bxCredit.value)
    bxTemp = Math.floor(bxTemp * 100) / 100
    res = bxTemp
  }
  return res
}

// 添加
const submit = () => {
  if (score.value === '') return
  const scoreArr = score.value.split(' ')

  const tempArr = scoreArr
    .filter(item => !!item)
    .map(item => {
      return {
        score: Number(item),
        credit: Number(creditSelected.value)
      }
    })

  if (courseSelected.value === '必修') {
    addSuject(bxArr, tempArr)
  } else {
    addSuject(xxArr, tempArr)
  }

  res.value = calReslust()
  console.log('res',res.value);
  
  score.value = ''
}

const clear = () => {
  bxArr.value = []
  xxArr.value = []
  bxScore.value = 0
  bxCredit.value = 0
  xxScore.value = 0
  xxCredit.value = 0
  res.value = 0
}

</script>

<style>
body,
html {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
}

input,
select {
  outline: none;
}

.app {
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.app h1 {
  margin: 6rem 0 4rem 0;
  font-size: 2.5rem;
}

.content {
  min-height: 30vh;
  width: 40%;
  max-width: 70vw;
  display: flex;
  flex-direction: column;
}

.content .header {
  margin: 1rem 0 0.5rem;
  display: flex;
  justify-content: space-between;
}

.content input {
  height: 2rem;
}

.header .select select,
.button {
  width: 3.5rem;
  height: 2rem;
  line-height: 2rem;
  text-align: center;
  font-weight: 600;
  font-size: 1rem;
}

.button {
  box-sizing: border-box;
  cursor: pointer;
  /* border-radius: 5px; */
  border: solid 1px #000;
}

select + select,
.button + .button {
  margin-left: 1rem;
}

.record {
  margin-top: 1.5rem;
  display: flex;
  flex-direction: row;
}

.reslust {
  margin-top: 1.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.reslust .text {
  /* margin-top: 1rem; */
  font-size: 3rem;
  font-weight: 500;
}

.reslust .details {
  cursor: pointer;
  text-decoration: underline;
}
</style>
