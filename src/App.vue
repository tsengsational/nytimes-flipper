<template>
  <div id="app">
    <Instructions/>
    <Nav :handleSelect="handleSelect" />
    <Carousel :articles="articles" :getMore="getMore" :resetPages="resetPages" />
    <Footer/>
  </div>
</template>

<script>
import Nav from './components/Nav'
import Carousel from './components/Carousel'
import config from './config'
import Footer from './components/Footer'
import Instructions from './components/Instructions'

var startArticles = [
  {
    headline: {
      main: ""
    },
    byline: "",
    snippet: "",
    multimedia: []
  },
  {
    headline: {
      main: ""
    },
    byline: "",
    snippet: "",
    multimedia: []
  },
  {
    headline: {
      main: ""
    },
    byline: "",
    snippet: "",
    multimedia: []
  },
  {
    headline: {
      main: ""
    },
    byline: "",
    snippet: "",
    multimedia: []
  }
]

export default {
  name: 'app',
  components: {
    Nav,
    Carousel,
    Footer,
    Instructions
  },
  watch: {
    selected: function (newSelect, oldSelect) {
        this.page = 0
        let url = this.getURL(newSelect)
        this.getArticles(url, "start")
    }
  },
  methods: {
    handleSelect: function (e) {
      let value = e.target.value
      this.selected = value
    },
    getURL: function(sectionName = "") {
      let url = config.endpoint
      url += "?"
      url += `&page=${this.page}`
      url += '&fq=source:("The New York Times")'
      if (sectionName.length > 0 && sectionName !== "all") {
        url += ` AND section_name:("${sectionName}")`
      } else {
        url += ' AND section_name:("Multimedia/Photos")'
      }
      return url += `&sort=newest&api-key=${this.apiKey}`
    },
    getArticles: function(url, callback = null) {
      return fetch(url)
        .then(response => {
          return response.json()
        })
        .then(json => {
          switch (callback) {
            case "start":
              this.resetPages = true
              this.populateStart(json);
              setTimeout(() => {
                this.resetPages = false
              }, 600);
              break;
            case "update":
              this.populateArticles(json);
              break;
            default:
              console.log('no callback')
              break;
          }
        })
    },
    populateArticles: function(json) {
      const articles = json.response.docs
      this.articles = this.articles.concat(articles)
    },
    populateStart: function(json) {
      const articles = json.response.docs
      this.articles = articles
    },
    getMore: function() {
      this.page++
      let url = this.getURL(this.selected)
      return this.getArticles(url, "update")
    }
  },
  data: function(){
    return {
      apiKey: config.articleSearch,
      selected: "",
      articles: startArticles,
      page: 0,
      resetPages: false
    }
  },
  created: function(){
    let url = this.getURL()
    this.getArticles(url, "start")
  }
}
</script>

<style lang="scss">
@import "assets/settings.scss";

#app {
  height: 100vh;
  overflow: hidden;
  font-family: $font_header;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

body {
  margin: 0;
}
</style>
