<script setup>
import draggable from 'vuedraggable'
import { ref } from 'vue'
import _ from 'lodash'

const questions = ref([
  {
    i: 1,
    // a: 'We don’t do our physical training',
    // value: ['do', 'physical', 'We', 'training', 'our', 'don’t'],
    a: 'Alexander Suvorov was a famous Russian general',
    value: ['famous', 'was', 'Alexander', 'a', 'Suvorov', 'general', 'Russian'],
    s: false,
    after: '.'
  },
  {
    i: 2,
    // a: 'When do you line up for the morning inspection',
    // value: ['you', 'inspection', 'for', 'When', 'morning', 'the', 'line', 'up', 'do'],
    a: 'He thought that a strong body was important for a strong mind',
    value: ['body', 'was', 'thought', 'strong', 'a', 'that', 'important', 'a', 'He', 'for', 'mind', 'strong'],
    s: false,
    after: '.'
  },
  {
    i: 3,
    // a: 'He has his meals four times a day',
    // value: ['has', 'his', 'He', 'a', 'day', 'four', 'meals', 'times'],
    a: 'Suvorov started his day early',
    value: ['his', 'Suvorov', 'day', 'early', 'started'],
    s: false,
    after: '.'
  },
  {
    i: 4,
    // a: 'Cadets have three hours of self-preparation and one hour of clubs',
    // value: ['three', 'hours', 'Cadets', 'of', 'self-preparation', 'of', 'clubs', 'and', 'have', 'one', 'hour'],
    a: 'He ate good food, like meat and vegetables',
    value: ['food,', 'He', 'like', 'vegetables', 'ate', 'good', 'and', 'meat'],
    s: false,
    after: '.'
  },
  {
    i: 5,
    // a: 'After the signal “lights out”, we go to bed',
    // value: ['we', 'to', 'the', 'signal', 'After', '“lights out”,', 'go', 'bed'],
    a: 'He worked, did exercises, and rested',
    value: ['and', 'exercises,', 'rested', 'did', 'worked', 'He'],
    s: false,
    after: '.'
  },
  {
    i: 6,
    // a: 'After the signal “lights out”, we go to bed',
    // value: ['we', 'to', 'the', 'signal', 'After', '“lights out”,', 'go', 'bed'],
    a: 'A good daily routine can help you to be successful',
    value: ['daily', 'good', 'help', 'to', 'you', 'can', 'A', 'successful', 'routine', 'be'],
    s: false,
    after: '.'
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
    item.s = item.a === str
    if (item.a !== str) {
      res = false
    }
  })
  return res
}
</script>

<template>
  <div class="fill d-flex flex-column wrapper">
    <div
      v-for="(item, idx) in questions"
      :key="item.i"
      class="d-flex flex-column"
      :class="{ solution: isSolved, green: item.s }"
    >
      <draggable class="sortable-list list-group" v-model="item.value" group="people" ghost-class="ghost" item-key="id">
        <template #item="{ element, index }" :key="index">
          <div class="word">
            {{ index < item.value.length - 1 ? element.replace('.', ',') : element.replace(',', '') + item.after }}
          </div>
        </template>
      </draggable>
      <!-- <div>{{ _.shuffle(item.value) }}</div> -->
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
  background: rgb(201 202 135);
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

.solution {
  padding: 0px 16px;
  border: 3px dashed red;
  border-radius: 4px;
  margin-bottom: 4px;
  transition-duration: 1s;
}

.solution.green {
  border-color: green;
}
</style>
