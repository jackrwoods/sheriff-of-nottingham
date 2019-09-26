<!--
@Author: Jack Woods
@Date:   2019-09-24T13:48:04-07:00
@Email:  jackrwoods@gmail.com
@Filename: HelloWorld.vue
@Last modified by:   Jack Woods
@Last modified time: 2019-09-25T19:23:19-07:00
-->

<template>
  <div class="main">
    <b>Unknown Cards Likely to Be in Play ({{ percentKnown.toFixed(2) }}% Known):</b><br />
    <span v-for="key in Object.keys(goodsUnaccountedFor)">
      {{ key }}: {{ ((numOfPlayers * 6 - numAccountedFor) * pdg(key)).toFixed(3) }}<br />
    </span>
    <br />
    <b>Known Goods:</b><br />
    <div v-for="(p, index) in playersHands">
      {{ index == 0 ? "You" : "Player " + index }}:
      <div v-for="g in Object.keys(playersHands[0])"> <!-- iterate through all goods -->
        <span v-if="p[g] > 0">
          {{ g }}: {{ p[g] }}
        </span>
      </div><br />
    </div>
    {{ avgErrorTest }}
  </div>
</template>

<script>
export default {
  name: 'CardCounter',
  data () {
    return {
      numOfPlayers: 5,
      playersHands: [], // An array of objects containing counts for known goods in a player's hand. Index 0 is aways the current player, ie: the user of this application
      goodsDiscarded: {
        // Legal Goods
        apple: 0,
        cheese: 0,
        bread: 0,
        chicken: 0,
        // Contraband
        pepper: 0,
        mead: 0,
        silk: 0,
        crossbow: 0,
        // Royal Goods
        royalRooster: 0,
        ryeBread: 0,
        pumpernickelBread: 0,
        goudaCheese: 0,
        bleuCheese: 0,
        greenApple: 0,
        goldenApple: 0
      },
      discardPile1: [],
      discardPile2: [],
      goodsPlayed: {
        // Legal Goods
        apple: 0,
        cheese: 0,
        bread: 0,
        chicken: 0,
        // Contraband
        pepper: 0,
        mead: 0,
        silk: 0,
        crossbow: 0,
        // Royal Goods
        royalRooster: 0,
        ryeBread: 0,
        pumpernickelBread: 0,
        goudaCheese: 0,
        bleuCheese: 0,
        greenApple: 0,
        goldenApple: 0
      }
    }
  },
  computed: {
    goodsUnaccountedFor() {
      return {
        apple: 48 - this.numOfGoodInPlay('apple'), // Legal Goods
        cheese: 36 - this.numOfGoodInPlay('cheese'),
        bread: 36 - this.numOfGoodInPlay('bread'),
        chicken: 24 - this.numOfGoodInPlay('chicken'),
        pepper: 22 - this.numOfGoodInPlay('pepper'), // Contraband
        mead: 21 - this.numOfGoodInPlay('mead'),
        silk: 12 - this.numOfGoodInPlay('silk'),
        crossbow: 5 - this.numOfGoodInPlay('crossbow'),
        royalRooster: 2 - this.numOfGoodInPlay('royalRooster'), // Royal Goods
        ryeBread: 2 - this.numOfGoodInPlay('ryeBread'),
        pumpernickelBread: 1 - this.numOfGoodInPlay('pumpernickelBread'),
        goudaCheese: 2 - this.numOfGoodInPlay('goudaCheese'),
        bleuCheese: 1 - this.numOfGoodInPlay('bleuCheese'),
        greenApple: 2 - this.numOfGoodInPlay('greenApple'),
        goldenApple: 2 - this.numOfGoodInPlay('goldenApple')
      }
    },
    // Total number of cards remaining in the deck
    cardsRemaining() {
      let sum = 0
      Object.keys(this.goodsUnaccountedFor).forEach(key => {
        sum += this.goodsUnaccountedFor[key]
      })
      return sum
    },
    numAccountedFor() {
      let sum = 0
      this.playersHands.forEach(p => {
        Object.keys(p).forEach(key => {
          sum += p[key]
        })
      })
      return sum
    },
    // What percentage of cards currently in play are known?
    // For example, if we only know at least 2 apples are in play, the percent is 2/30 * 100 = 6.67%
    percentKnown() {
      return this.numAccountedFor / (6 * this.numOfPlayers) * 100
    },
    avgErrorTest() {
      // Create an object representing expected results
      const EXPECTED = {
        // Legal Goods
        apple: this.pdg('apple'),
        cheese: this.pdg('cheese'),
        bread: this.pdg('bread'),
        chicken: this.pdg('chicken'),
        // Contraband
        pepper: this.pdg('pepper'),
        mead: this.pdg('mead'),
        silk: this.pdg('silk'),
        crossbow: this.pdg('crossbow'),
        // Royal Goods
        royalRooster: this.pdg('royalRooster'),
        ryeBread: this.pdg('ryeBread'),
        pumpernickelBread: this.pdg('pumpernickelBread'),
        goudaCheese: this.pdg('goudaCheese'),
        bleuCheese: this.pdg('bleuCheese'),
        greenApple: this.pdg('greenApple'),
        goldenApple: this.pdg('goldenApple')
      }
      // Create array of "cards"
      let arr = []
      Object.keys(this.goodsUnaccountedFor).forEach(key => {
        for (let i = 0; i < this.goodsUnaccountedFor[key]; i++) {
          arr.push(key)
        }
      })

      // Shuffle the deck numTests times and calculate the per-card error
      // Compute the average per-card error and add it to the results array
      let results = []
      for (let numTests = 0; numTests < 100; numTests++) {

        // Random shuffle
        arr.sort(() => Math.random() - 0.5)
        let counts = {
          // Legal Goods
          apple: 0,
          cheese: 0,
          bread: 0,
          chicken: 0,
          // Contraband
          pepper: 0,
          mead: 0,
          silk: 0,
          crossbow: 0,
          // Royal Goods
          royalRooster: 0,
          ryeBread: 0,
          pumpernickelBread: 0,
          goudaCheese: 0,
          bleuCheese: 0,
          greenApple: 0,
          goldenApple: 0
        }

        // Tally the counts of the first 30 cards
        for (let j = 0; j < 30; j++) {
          counts[arr[j]]++
        }

        // Subtract the expected value
        Object.keys(counts).forEach(key => {
          counts[key] -= EXPECTED[key]
        })

        // Add all errors together
        let sum = 0;
        Object.keys(counts).forEach(key => {
          sum += counts[key]
        })

        results.push(parseFloat((sum / 30).toFixed(5)))
      }

      return JSON.stringify(results)
    }
  },
  methods: {
    // Probability that a particular good could be in play
    pdg(nameOfGood) {
      return (this.goodsUnaccountedFor[nameOfGood] - this.goodsPlayed[nameOfGood] - this.goodsDiscarded[nameOfGood]) / (this.cardsRemaining)
    },
    // Returns the number of a certain good that are known to be currently in play
    numOfGoodInPlay(nameOfGood) {
      let sum = 0
      this.playersHands.forEach(p => {
        sum += p[nameOfGood]
      })
      return sum
    }
  },
  created() {
    for (let i = 0; i < this.numOfPlayers; i++) {
      // Initially, none are known
      this.playersHands.push({
        apple: 0,
        cheese: 0,
        bread: 0,
        chicken: 0,
        pepper: 0,
        mead: 0,
        silk: 0,
        crossbow: 0,
        royalRooster: 0,
        ryeBread: 0,
        pumpernickelBread: 0,
        goudaCheese: 0,
        bleuCheese: 0,
        greenApple: 0,
        goldenApple: 0
      })
      this.playersHands[0] = {
          apple: 0,
          cheese: 0,
          bread: 0,
          chicken: 0,
          pepper: 0,
          mead: 0,
          silk: 0,
          crossbow: 0,
          royalRooster: 0,
          ryeBread: 0,
          pumpernickelBread: 0,
          goudaCheese: 0,
          bleuCheese: 0,
          greenApple: 0,
          goldenApple: 0
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
