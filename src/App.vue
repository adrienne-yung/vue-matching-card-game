<template>
  <h1>Summer Vue</h1>
    <section class="game-board">
    <Card v-for="(card, index) in cardList"
    :key="`card-${index}`"
    :matched="card.matched"
    :value="card.value"
    :visible="card.visible"
    :position="card.position"
    @select-card="flipCard"
    />
    </section>
    <h2>{{ status }}</h2>
</template>

<script>
import { ref, watch } from 'vue'
import Card from './components/Card-File'

export default {
  name: 'App',
  components: {
    Card
  },
  setup() {
    const cardList = ref([])
    const userSelect = ref([])
    const status = ref('')

    for (let i = 0; i< 16; i++) {
      cardList.value.push({
        value: i,
        visible: false,
        position: i,
        matched: false
      })
    }

    const flipCard = (payload) => {
      cardList.value[payload.position].visible = true

      if (userSelect.value[0]) {
        userSelect.value[1] = payload
      } else {
        userSelect.value[0] = payload
      }
    }

    watch(userSelect, currentValue => {
      if (currentValue.length === 2) {
        const cardOne = currentValue[0]
        const cardTwo = currentValue[1]

        if (cardOne.faceValue === cardTwo.faceValue) {
          status.value = "Matched!"
          cardList.value[cardOne.position].matched = true
          cardList.value[cardTwo.position].matched = true
        } else {
          status.value = "Mismatched!"
          cardList.value[cardOne.position].visible = false
          cardList.value[cardTwo.position].visible = false
        }


        userSelect.value.length = 0
      }
    },
    { deep: true }
    )
    return {
      cardList,
      flipCard,
      userSelect,
      status
    }
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}


.game-board {
  display: grid;
  grid-template-columns: 100px 100px 100px 100px;
  grid-column-gap: 30px;
  grid-template-rows: 100px 100px 100px 100px;
  grid-row-gap: 30px;
  justify-content: center;
}
</style>
