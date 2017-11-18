<template>
  <v-app light>
    <v-container grid-list-xl text-xs-center>
      <v-layout row>
        <v-flex xs4 offset-xs4>
          <v-btn  color="blue" fab dark large top center @click="getWeather()">
            <v-icon>cloud_queue</v-icon>
          </v-btn>
        </v-flex>
      </v-layout>
      <v-layout v-show="descriptionVisible" row>
        <v-flex sm6 offset-sm3 xs12>
          <v-card dark tile flat color="error">
            <v-card-text>{{ description }}</v-card-text>
          </v-card>
        </v-flex>
      </v-layout>
      <v-layout v-show="umbrellaVisible" row>
        <v-flex sm6 offset-sm3 xs12>
          <v-card dark tile flat :color="tileColor">
            <v-card-text>{{ umbrella }}</v-card-text>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
    <v-layout row>
      <v-flex xs12>
        <v-footer class="pa-3" absolute bottom>
          <v-spacer></v-spacer>
          <div>© {{ new Date().getFullYear() }}</div>
        </v-footer>
      </v-flex>
    </v-layout>
  </v-app>
</template>

<script>
export default {
  data () {
    return {
      descriptionVisible: false,
      umbrellaVisible: false,
      tileColor: '',
      umbrella: '',
      description: '',
      key: 'apikey=gn5l8vw37h1p5ao6sozdSJ7GlNYBdF30&',
      city: 'q=chicago&',
      language: 'language=en-us',
      locatonURL: 'http://dataservice.accuweather.com/locations/v1/cities/autocomplete?',
      weatherURL: 'http://dataservice.accuweather.com/forecasts/v1/daily/1day/348308?apikey=gn5l8vw37h1p5ao6sozdSJ7GlNYBdF30&language=en-us&details=false&metric=false',
      locationKey: '348308' // for Chicago
    }
  },
  methods: {
    // getLocation () {
    //   // fetches array of location based on autocomplete input
    //   fetch(this.locatonURL + this.key + this.city + this.language)
    //     .then((resp) => resp.json())
    //     .then(function (data) {
    //       console.log(data)
    //       // Your code for handling the data you get from the API
    //     })
    //     .catch(function (error) {
    //       console.log(error)
    //     })
    // },
    getWeather () {
      let self = this
      fetch(this.weatherURL)
        .then((resp) => resp.json())
        .then(function (data) {
          self.init()
          let condition = data.Headline.Category
          let effectiveTime = data.Headline.EffectiveEpochDate
          let currentTime = new Date() / 1000
          if (currentTime >= effectiveTime && (condition === 'rain' || condition === 'thunderstorm')) {
            self.umbrella = 'Better Grab an Umbrella!'
            self.description = data.Headline.Text
            self.umbrellaVisible = true
            self.descriptionVisible = true
            self.tileColor = 'red'
          } else {
            self.umbrella = 'No Umbrella Today'
            self.tileColor = 'green'
            self.umbrellaVisible = true
          }
        })
        .catch(function (error) {
          console.log(error)
          alert(error, 'error fetching data')
        })
    },
    init () {
      this.umbrellaVisible = false
      this.descriptionVisible = false
    }
  }
}
</script>

<style lang="stylus">
  @import './stylus/main'
</style>
