<template>
  <div class="container">
    <div class="img"></div>
    <div class="img"><img :src="`${img}`"></div>
    <div class="img"></div>
    <div class="buttons">
      <button class="btn" @click="getImage">test 1</button>
      <button class="btn">test 2</button>
      <button class="btn">test 3</button>
      <button class="btn">test 4</button>
    </div>
    <div class="stats">
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'MainGame',
  props: {
    msg: String
  },
  data: function () {
    return {
      breedList: [],
      breeds: [],
      breedsWithSubs: [],
      img: null,
      currentName: null
    }
  },
  methods: {
    getImage () {
      var randomBreed = null
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
    })
    .catch((error) => {
      console.error(error)
    })
  }
}
</script>

<style>
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
