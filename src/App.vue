<template>
  <h1 class="header-margin">Summer Vue</h1>
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
    <button @click="restartGame">Restart Game</button>
</template>

<script>
import _ from 'lodash'
import { computed, ref, watch } from 'vue'
import Card from './components/Card-File'

export default {
  name: 'App',
  components: {
    Card
  },
  setup() {
    const cardList = ref([])
    const userSelect = ref([])
    const status = computed(() => {
      if (remainingPairs.value === 0) {
        return 'You win!'
      } else {
        return `Remaining Pairs: ${remainingPairs.value}`
      }
    })

    const remainingPairs = computed(() => {
      const remainingCards = cardList.value.filter(card => card.matched === false).length

      return remainingCards / 2
    })

    const shuffleCards = () => {
      cardList.value = _.shuffle(cardList.value)
    }

    const restartGame = () => {
      shuffleCards()
      cardList.value = cardList.value.map((card, index) => {
        return {
          ...card,
          matched: false,
          visible: false,
          position: index
        }
      })
    }

    const cardItems = ['waves-1', 'waves-2', 'waves-3', 'waves-4', 'waves-5', 'waves-6', 'waves-7', 'waves-8']
    cardItems.forEach(item => {
      cardList.value.push({
        value: item,
        visible: false,
        position: null,
        matched: false
      })

      cardList.value.push({
        value: item,
        visible: false,
        position: null,
        matched: false
      })
    })

    cardList.value = cardList.value.map((card, index) => {
      return {
        ...card,
        position: index
      }
    })

    const flipCard = (payload) => {
      cardList.value[payload.position].visible = true

      if (userSelect.value[0]) {
        if ((userSelect.value[0].position === payload.position) && (userSelect.value[0].faceValue === payload.faceValue)) {
          return
        } else {
          userSelect.value[1] = payload
        }
      } else {
        userSelect.value[0] = payload
      }
    }

    watch(userSelect, currentValue => {
      if (currentValue.length === 2) {
        const cardOne = currentValue[0]
        const cardTwo = currentValue[1]

        if (cardOne.faceValue === cardTwo.faceValue) {

          cardList.value[cardOne.position].matched = true
          cardList.value[cardTwo.position].matched = true
        } else {
          setTimeout(() => {
          cardList.value[cardOne.position].visible = false
          cardList.value[cardTwo.position].visible = false
          }, 2000)

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
      status,
      shuffleCards,
      restartGame
    }
  }

}
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-image: url('assets/images/beachbackground.jpg');
  background-repeat: no-repeat;
  background-position: center;
  background-size: 100vw 100vh;
  width: 100vw;
  height: 100vh;
}


.game-board {
  display: grid;
  grid-template-columns: repeat(4, 120px);
  grid-column-gap: 25px;
  grid-template-rows: repeat(4, 120px);
  grid-row-gap: 25px;
  justify-content: center;
}

h1 {
  margin-top: 0;
  padding-top: 5%;
}
</style>
