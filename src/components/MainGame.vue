<template>
  <div class="container">
    <div class="score"></div>
    <div class="score">
      <h1>Score: {{score}}</h1>
    </div>
    <div class="score"></div>
    <div class="img"></div>
    <div class="img"><img :src="`${img}`"></div>
    <div class="img"></div>
    <failModalVue v-show="isModalVisible" @close="closeModal" :score="scoreTemp" :correctAnswer="answer" />
    <div class="buttons">
      <button class="btn" @click="guess(1)">{{valueOne}}</button>
      <button class="btn" @click="guess(2)">{{valueTwo}}</button>
      <button class="btn" @click="guess(3)">{{valueThree}}</button>
      <button class="btn" @click="guess(4)">{{valueFour}}</button>
    </div>
    <div class="stats">
    </div>
  </div>
</template>

<script>
import failModalVue from './failModal.vue'
import axios from 'axios'
export default {
  name: 'MainGame',
  components: {
    failModalVue
  },
  props: {
    msg: String
  },
  data: function () {
    return {
      breedList: [],
      breeds: [],
      breedsWithSubs: [],
      img: null,
      currentName: null,
      tempValueOne: null,
      tempValueTwo: null,
      tempValueThree: null,
      tempValueFour: null,
      correct: null,
      score: 0,
      isModalVisible: false,
      correctAnswerTemp: "test",
      scoreTemp: ''
    }
  },
  computed: {
    valueOne () {
      return this.tempValueOne.charAt(0).toUpperCase(0) + this.tempValueOne.slice(1)
    },
    valueTwo () {
      return this.tempValueTwo.charAt(0).toUpperCase(0) + this.tempValueTwo.slice(1)
    },
    valueThree () {
      return this.tempValueThree.charAt(0).toUpperCase(0) + this.tempValueThree.slice(1)
    },
    valueFour () {
      return this.tempValueFour.charAt(0).toUpperCase(0) + this.tempValueFour.slice(1)
    },
    answer () {
      return this.correctAnswerTemp.charAt(0).toUpperCase(0) + this.correctAnswerTemp.slice(1)
    }
  },
  methods: {
    guess (isCorrect) {
      if (isCorrect === this.correct) {
        this.score++
        this.getImage()
      } else {
        this.scoreTemp = this.score
        this.score = 0
        this.showModal()
      }

    },
    showModal() {
      this.isModalVisible = true;
      this.correctAnswerTemp = this.currentName
    },
    closeModal() {
      this.isModalVisible = false;
      if (this.isModalVisible === false) {
        this.getImage ()
      }
    },
    getImage () {
      var randomBreed = null
      var randomCorrectSlot = parseInt(Math.random() * ((4) - 1 + 1), 10)  + 1
      var randomInt = parseInt(Math.random() * ((this.breeds.length - 1) - 0 + 1), 10)  + 0
      axios.get('https://dog.ceo/api/breed/' + this.breeds[randomInt] + '/list').then((res) => {
        if (res.data.message.length === 0) {
            axios.get('https://dog.ceo/api/breed/' +this.breeds[randomInt]+ '/images/random').then(res => this.img = res.data.message)
            this.currentName = this.breeds[randomInt]
        } else {
          randomBreed = parseInt(Math.random() * ((res.data.message.length - 1) - 0 + 1), 10)  + 0
          axios.get('https://dog.ceo/api/breed/' +this.breeds[randomInt]+'/'+res.data.message[randomBreed]+'/images/random').then(res => this.img = res.data.message)
          this.currentName = res.data.message[randomBreed] + ' ' + this.breeds[randomInt]
        }
        var rand2 = parseInt(Math.random() * ((this.breeds.length - 1) - 0 + 1), 10)  + 0
        var rand3 = parseInt(Math.random() * ((this.breeds.length - 1) - 0 + 1), 10)  + 0
        var rand4 = parseInt(Math.random() * ((this.breeds.length - 1) - 0 + 1), 10)  + 0
        switch (randomCorrectSlot) {
          case 1:
            this.correct = 1
            this.tempValueOne = this.currentName
            if (this.tempValueOne != this.breeds[rand2]) {
              this.tempValueTwo = this.breeds[rand2]
            } else {
              rand2 = parseInt(Math.random() * ((this.breeds.length - 1) - 0 + 1), 10)  + 0
            }
            if (this.tempValueOne != this.breeds[rand3]) {
              this.tempValueThree = this.breeds[rand3]
            } else {
              rand3 = parseInt(Math.random() * ((this.breeds.length - 1) - 0 + 1), 10)  + 0
            }
            if (this.tempValueOne != this.breeds[rand4]) {
              this.tempValueFour = this.breeds[rand4]
            } else {
              rand4 = parseInt(Math.random() * ((this.breeds.length - 1) - 0 + 1), 10)  + 0
            }
            break
          case 2:
            this.correct = 2
            this.tempValueTwo = this.currentName
            if (this.tempValueTwo != this.breeds[rand2]) {
              this.tempValueOne = this.breeds[rand2]
            } else {
              rand2 = parseInt(Math.random() * ((this.breeds.length - 1) - 0 + 1), 10)  + 0
            }
            if (this.tempValueTwo != this.breeds[rand3]) {
              this.tempValueThree = this.breeds[rand3]
            } else {
              rand3 = parseInt(Math.random() * ((this.breeds.length - 1) - 0 + 1), 10)  + 0
            }
            if (this.tempValueTwo != this.breeds[rand4]) {
              this.tempValueFour = this.breeds[rand4]
            } else {
              rand4 = parseInt(Math.random() * ((this.breeds.length - 1) - 0 + 1), 10)  + 0
            }
            break
          case 3:
            this.correct = 3
            this.tempValueThree = this.currentName
            if (this.tempValueThree != this.breeds[rand2]) {
              this.tempValueTwo = this.breeds[rand2]
            } else {
              rand2 = parseInt(Math.random() * ((this.breeds.length - 1) - 0 + 1), 10)  + 0
            }
            if (this.tempValueThree != this.breeds[rand3]) {
              this.tempValueOne = this.breeds[rand3]
            } else {
              rand3 = parseInt(Math.random() * ((this.breeds.length - 1) - 0 + 1), 10)  + 0
            }
            if (this.tempValueThree != this.breeds[rand4]) {
              this.tempValueFour = this.breeds[rand4]
            } else {
              rand4 = parseInt(Math.random() * ((this.breeds.length - 1) - 0 + 1), 10)  + 0
            }
            break
          case 4:
            this.correct = 4
            this.tempValueFour = this.currentName
            if (this.tempValueFour != this.breeds[rand2]) {
              this.tempValueTwo = this.breeds[rand2]
            } else {
              rand2 = parseInt(Math.random() * ((this.breeds.length - 1) - 0 + 1), 10)  + 0
            }
            if (this.tempValueFour != this.breeds[rand3]) {
              this.tempValueThree = this.breeds[rand3]
            } else {
              rand3 = parseInt(Math.random() * ((this.breeds.length - 1) - 0 + 1), 10)  + 0
            }
            if (this.tempValueFour != this.breeds[rand4]) {
              this.tempValueOne = this.breeds[rand4]
            } else {
              rand4 = parseInt(Math.random() * ((this.breeds.length - 1) - 0 + 1), 10)  + 0
            }
            break
        }
      })
    }
  },
  created () {
    axios.get('https://dog.ceo/api/breeds/list/all').then((res) => {
      this.breedList = res.data.message
      Object.keys(this.breedList).forEach(breed => this.breeds.push(breed))
      this.breeds.forEach(breed => {
        axios.get('https://dog.ceo/api/breed/' + breed + '/list').then((res) => {
          if (res.data.message.length === 0) {
            this.breedsWithSubs.push(breed)
          } else {
            res.data.message.forEach(sub => {
                this.breedsWithSubs.push(sub + ' ' + breed)
            })
          }
        })
      })
      this.getImage()
    })
    .catch((error) => {
      console.error(error)
    })
  }
}
</script>

<style>
.score {
  flex: 0 0 33%;
  color: aqua;
  justify-content: center;
  display: flex;
}
.img {
  flex: 0 0 33%;
}
.stats {
  flex: 0 0 100%;
}
img {
  object-fit: cover;
  width: 100%;
  height: 700px;
}
.container {
  margin: 5%;
  justify-content: center;
  display: flex;
  flex-wrap: wrap;
  background-color: black;
}
.buttons {
  margin: 2%;
  justify-content: center;
  display: flex;
  flex-wrap: wrap;
  width: 600px;

}
.btn {
    flex: 0 0 48%;
    outline: none;
    background: none;
    color: white;
    border: 1px solid aqua;
    padding: 10px 20px;
    margin-right: 10px;
}
.btn:hover {
  cursor: pointer;
  color: white;
  border: 1px solid aqua;
  background-color: aqua;
}
</style>
