<template>
  <q-page v-touch-pan.prevent.mouse="handlePan" class="flex flex-center">
    <div class="row">
      <q-input v-model="data.name" input-class="text-center text-h5 text-white" color="teal" filled
        placeholder="Counter" />
    </div>

    <div class="row full-width text-white items-center">
      <div class="col text-center">
        <q-btn v-touch-repeat:300:300:300:300:50.mouse="decreaseCounter" round icon="remove" size="xl"
          @click="decreaseCounter" />
      </div>
      <div class="col text-center text-h2">
        {{ data.counter }}
      </div>
      <div class="col text-center">
        <q-btn @click="increaseCounter" v-touch-repeat:300:300:300:300:50.mouse="increaseCounter" round icon="add"
          size="xl" />
      </div>
    </div>

    <div class="row">
      <div class="col text-center">
        <q-btn round icon="restart_alt" size="xl" @click="resetCounter" />
      </div>
    </div>
  </q-page>
</template>

<script setup>
/*
  imports
*/
import { useQuasar } from 'quasar';
import { reactive, watch } from 'vue';
const $q = useQuasar()
/*
  data
*/
const data = reactive({
  counter: 0,
  name: ""
})

const saveData = $q.localStorage.getItem('data')

if (saveData)
  Object.assign(data, saveData)

watch(data, value => {
  // console.log(value)
  $q.localStorage.set('data', value)
})

/*
  counter methods
*/
const increaseCounter = () => {
  data.counter++
}
const decreaseCounter = () => {
  if (data.counter > 0) {
    data.counter--
  }
}
const resetCounter = () => {
  data.counter = 0
}

const handlePan = e => {
  console.log(e.delta.y)
  if (e.delta.y < 0) {
    increaseCounter()
  } else {
    decreaseCounter()
  }
}
</script>


<style scoped>
.q-page {
  max-width: 700px;
  margin: 0;
}
</style>