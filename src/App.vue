<template>
  <h1>Card Memory Game</h1>
  <transition-group tag="section" class="game-board" name="shuffle-card">
    <Card 
    v-for="card in cardList"
    :key="`${card.value}-${card.variant}`"
    :matched="card.matched"
    :value="card.value"
    :visible="card.visible"
    :position="card.position"
    @select-card="flipCard"
    />
  </transition-group>
  <h2>{{ status }}</h2>
  <button @click="restartGame" class="button" ><img src="Images/restart.svg" alt="Restart Icon"/> Restart Game </button>
</template>

<script>
import _ from 'lodash'
import { computed, ref, watch } from 'vue'
import Card from './components/Card.vue'

export default {
  name: 'App',
  components: {
    Card
  },
  setup() {
    const cardList = ref([])
    const userSelection = ref([])
    const status = computed(() => {
      if (remainingPairs.value === 0) {
        return 'Player wins!'
      } else {
        return `Remaining Pairs: ${remainingPairs.value}`
      }
    })
    const remainingPairs = computed(() => {
      const remainingCards = cardList.value.filter(card => card.matched === false).length

      return remainingCards / 2
    })

    const restartGame = () => {
      cardList.value = _.shuffle(cardList.value)

      cardList.value = cardList.value.map((card, index) => {
        return {
          ...card,
          matched: false,
          position: index,
          visible: false
        }
      })
    }

    const cardItems = ['Bat', 'Bones', 'Dracula', 'Eye', 'Ghost', 'Pumpkin', 'Skull', 'Cauldron']

    cardItems.forEach(item => {
      cardList.value.push({
        value: item,
        variant: 1,
        visible: false,
        position: null,
        matched: false
      })

      cardList.value.push({
        value: item,
        variant: 2,
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

      if (userSelection.value[0]) {
        if (
          userSelection.value[0].position === payload.position &&
           userSelection.value[0].faceValue === payload.faceValue
           ) {
             return
        } else {
          userSelection.value[1] = payload
        }
      } else {
        userSelection.value[0] = payload
      }
    }

    watch(userSelection, currentValue => {
      if (currentValue.length === 2) {
        const firstCard = currentValue[0]
        const secondCard = currentValue[1]

        if (firstCard.faceValue === secondCard.faceValue) {
          cardList.value[firstCard.position].matched = true
          cardList.value[secondCard.position].matched = true
        } else {
          setTimeout(() => {
            cardList.value[firstCard.position].visible = false
            cardList.value[secondCard.position].visible = false
          }, 2000)
        }


        userSelection.value.length = 0
      }
    },
    { deep: true}
    )

    return {
      cardList,
      flipCard,
      userSelection,
      status,
      restartGame
    }
  }
}
</script>

<style>
@font-face{
  font-family: "Creepy";
  src: url('../public/Fonts/Creepy.woff') format('woff'), url('../public/Fonts/Creepy.woff2') format('woff2')
}
html, body{
  margin: 0;
  padding: 0;
}

h1{
  margin-top: 0;
  font-family: Creepy, serif;
  font-weight: normal;
  text-align: center;
  font-size: 40px;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-image: url('../public/Images/page-bg.png');
  background-color: #573f21;
  height: 100vh;
  color: #FF6D00;
  padding-top: 10px;
}

.game-board{
  display: grid;
  grid-template-columns: 125px 125px 125px 125px;
  grid-column-gap: 24px;
  grid-template-rows: 125px 125px 125px 125px;
  grid-row-gap: 24px;
  justify-content: center;
}

.button {
  background-color: #FF6D00;
  color: #fff;
  padding: 0.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
  border-radius: 10px;
}
.button img {
  padding-right: 5px;
  font-weight: bold;
}
.shuffle-card-move {
  transition: transform 0.8s ease-in;
}
</style>
