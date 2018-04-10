<template lang="html">
  <div class="carousel">
    <div class="first" >
      <articleCard class="top" :article="this.topArticle"/>
    </div>
    <div class="pages" >
      <page v-for="(page, key) in pageArticles" v-if="key % 2 === 0" @pageFlip="handlePageFlip" :index="key" :resetPages="resetPages" :indexViewed="indexViewed" :articles="pageArticles" />
    </div>
    <div class="hidden">
      <articleCard v-for="(article, key) in articles" />
    </div>
  </div>
</template>

<script>
import ArticleCard from "./Article"
import Page from "./Page"

export default {
  props: ["articles", "getMore", 'resetPages'],
  components: {
    ArticleCard,
    Page
  },
  data: function() {
    return {
      indexViewed: 0,
      }
  },
  computed: {
    topArticle: function() {
      return this.articles[0]
    },
    bottomArticle: function() {
      return this.articles[this.articles.length - 1]
    },
    pageArticles: function() {
      let pageArticles = this.articles.slice(1, this.articles.length - 1)
      return pageArticles
    }
  },
  methods: {
    advance: function() {
     this.getMore()
   },
    handlePageFlip: function(index) {
      if (index >= (this.articles.length - 6)) {
        this.advance()
     }
   }
  }
}
</script>

<style lang="scss">
  @import "../assets/settings.scss";

  .hidden {
    opacity: 0;
  }
  .carousel {
    box-sizing: border-box;
    position: relative;
    height: $article_height;
  }
  .first {
    position: absolute;
    top: 0;
    left: 0;
  }

  @media (min-width: 450px) {
    .first {
      top: 0;
      left: 0;
    }
  }
</style>
