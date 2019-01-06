<template lang="pug" >
#app
  img(src='./assets/logo.png')
  h1 CoffeeMusic
  select(v-model="selectedCountry")
    option(v-for="country in countries" v-bind:value="country.value") {{ country.name }}
  spinner(v-show="Loading")
  ul
    artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist")

</template>

<script>
import Artist from './components/Artist.vue'
import getArtists from './api'
import Spinner from './components/Spinner.vue'
export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries:[
        { name: 'México', value: 'Mexico'},
        { name: 'España', value: 'spain'},
        { name: 'Colombia', value: 'Colombia'},
        { name: 'Argentina', value: 'Argentina'},
      ],
      selectedCountry: 'Mexico',
      Loading: true
      }
    },
    components: {
      Artist,
      Spinner
    },
    methods:{
      refreshArtist() {
      const self = this
      this.Loading = true
      getArtists(this.selectedCountry)
        .then(function (artists) {
          self.Loading = false
          self.artists = artists
      })
      }
    },
    mounted: function () {
      this.refreshArtist()
    },
    watch: {
     selectedCountry(){
        this.refreshArtist()
      }
    }

}
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color blue
  margin-top 60px

h1, h2
  font-weight normal

ul
  list-style-type none
  padding 0
a
  color red
</style>
