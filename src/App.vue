<script setup>
import draggable from 'vuedraggable'
import { ref } from 'vue'
const questions = ref([
  {
    i: 1,
    a: 'We do our physical training',
    value: ['do', 'physical', 'We', 'training', 'our']
  },
  {
    i: 2,
    a: 'We line up for the morning inspection',
    value: ['inspection', 'for', 'We', 'morning', 'the', 'line', 'up']
  },
  {
    i: 3,
    a: 'We have our meals four times a day',
    value: ['have', 'our', 'We', 'a', 'day', 'four', 'meals', 'times']
  },
  {
    i: 4,
    a: 'Cadets have three hours of self-preparation and one hour of clubs',
    value: ['three', 'hours', 'Cadets', 'of', 'self-preparation', 'of', 'clubs', 'and', 'have', 'one', 'hour']
  },
  {
    i: 5,
    a: 'After the signal “lights out”, we go to bed',
    value: ['we', 'to', 'the', 'signal', 'After', '“lights out”,', 'go', 'bed']
  }
])

const isSolved = ref(false)
const isCorrect = ref(false)

function checkClick() {
  isSolved.value = true
  isCorrect.value = getResult()
}

function getResult() {
  let res = true
  questions.value.forEach((item) => {
    let str = item.value.join(' ')
    if (item.a !== str) {
      res = false
    }
  })
  return res
}
</script>

<template>
  <div class="fill d-flex flex-column wrapper">
    <div v-for="(item, idx) in questions" :key="item.i" class="d-flex flex-column">
      <draggable class="sortable-list list-group" v-model="item.value" group="people" ghost-class="ghost" item-key="id">
        <template #item="{ element, index }" :key="index">
          <div class="word">
            {{ element }}
          </div>
        </template>
      </draggable>
    </div>
    <div class="d-flex flex-column">
      <div class="result" v-if="isSolved">
        <div class="correct" v-if="isCorrect">&#10004;</div>
        <div class="incorrect" v-else>&#10006;</div>
      </div>
      <button class="btn" @click="checkClick">Check</button>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  position: fixed;
  top: 0;
  left: 0;
  background: rgb(171, 202, 135);
  font-size: 200%;
}
.fill {
  width: 100%;
  height: 100%;
}

.d-flex {
  display: flex;
  align-items: center;
  justify-content: center;
}

.flex-column {
  flex-direction: column;
}
.sortable-list {
  display: flex;
  margin: 20px 0;
  flex-wrap: wrap;
}

.word {
  font-size: 120%;
  margin-right: 8px;
  background: white;
  user-select: none;
  box-shadow: 2px 2px 8px black;
}

.word:first-child::first-letter {
  text-transform: uppercase;
}

.word {
  cursor: move;
  padding: 0px 10px;
  border-radius: 2px;
}

.word:hover {
  background: rgb(169, 248, 169);
  box-shadow: 2px 2px 2px grey;
}

.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}

.item-pre {
  display: inline-block;
  font-size: 120%;
  text-decoration: underline;
}

.correct {
  color: green;
}

.incorrect {
  color: red;
}

.btn {
  margin-top: auto;
  margin-bottom: 2%;
  background-color: white; /* Green */
  border: none;
  color: black;
  padding: 8px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  transition-duration: 0.2s;
  cursor: pointer;
  border: 2px solid #04aa6d;
  box-shadow: 2px 2px 8px black;
}

.btn:hover {
  background-color: #04aa6d;
  color: white;
}

.result {
  margin: 20px;
}
</style>
