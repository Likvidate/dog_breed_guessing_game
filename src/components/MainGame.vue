<template>
  <div>
    <button @click="getData">test</button>
    <button @click="getImage">test</button>
    <div>Amount of breeds: {{breedsWithSubs.length}}</div>
    {{currentName}}
    <img :src="`${img}`">
    <ul v-for="breed in breedsWithSubs" v-bind:key="breed">
      <li>{{breed}}</li>
    </ul>
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
    getData () {
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
    },
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
  }
}
</script>

<style>
img {
  object-fit: cover;
  width: 50%;
  height: 700px;
}
</style>
