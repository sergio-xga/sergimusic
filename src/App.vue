<template lang='pug'>
  //- <div id="app">
  //-   <img src="./assets/logo.png">
  //-   <h1>{{ msg }}</h1>
  //-   <h2>Essential Links</h2>
  //-   <ul>
  //-     <li><a href="https://vuejs.org" target="_blank">Core Docs</a></li>
  //-     <li><a href="https://forum.vuejs.org" target="_blank">Forum</a></li>
  //-     <li><a href="https://chat.vuejs.org" target="_blank">Community Chat</a></li>
  //-     <li><a href="https://twitter.com/vuejs" target="_blank">Twitter</a></li>
  //-   </ul>
  //-   <h2>Ecosystem</h2>
  //-   <ul>
  //-     <li><a href="http://router.vuejs.org/" target="_blank">vue-router</a></li>
  //-     <li><a href="http://vuex.vuejs.org/" target="_blank">vuex</a></li>
  //-     <li><a href="http://vue-loader.vuejs.org/" target="_blank">vue-loader</a></li>
  //-     <li><a href="https://github.com/vuejs/awesome-vue" target="_blank">awesome-vue</a></li>
  //-   </ul>
  //- </div>
  #app
    img(src='./assets/logo.png')
    h1 SergiMusic
    select(v-model="selectedCountry")
      option(v-for="country in countries" v-bind:value="country.value") {{ country.name }}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
</template>

<script>
import getArtists from './api'
import Artist from './components/Artist.vue'
import Spinner from './components/Spinner.vue'
export default {
  name: 'app',
  data () {
    return {
      artists: [
      ],
      countries: [
        {name: 'United States', value: 'united states'},
        {name: 'Mexico', value: 'mexico'},
        {name: 'España', value: 'spain'}
      ],
      selectedCountry: "united states",
      loading: true,
    }
  },
  components: {
    Artist: Artist,
    Spinner: Spinner
  },
  mounted: function(){
    this.refreshArtists();
  },
  methods: {
    refreshArtists(){
      const self = this;
      this.loading = true;
      getArtists(this.selectedCountry)
        .then(function(artists){
          self.loading = false;
          self.artists = artists;
        })
    }
  },
  watch: {
    selectedCountry: function(){
      this.refreshArtists();
    }
  }
}
</script>

<style lang='stylus'>
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px

h1, h2
  font-weight normal

ul
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983
</style>
