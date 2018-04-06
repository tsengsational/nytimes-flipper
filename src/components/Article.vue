<template lang="html">
  <div class="article">
    <div class="headline">
      <div class="image" v-if="hasMultimedia" v-bind:style="imageStyle" >
      </div>
      <span v-html="this.article.headline.main" ></span>
    </div>
    <div class="byline">
      {{this.article.byline.original}}
    </div>
    <div class="snippet">
      <span class="text" v-html="this.article.snippet" ></span><span><a :href="article.web_url" >Read more...</a></span>
    </div>
  </div>
</template>

<script>
export default {
  props: ["article"],
  data: function(){
    return {

    }
  },
  computed: {
    hasMultimedia: function(){
      if (this.article.multimedia) {
        return this.article.multimedia.length > 0 ? true : false;
      } else {
        return false
      }
    },
    largeImage: function() {
      return this.article.multimedia.find(function(element){
        return element.crop_name === "articleLarge"
      });
    },
    thumbImage: function() {
      return this.article.multimedia.find(function(element){
        return element.crop_name === "thumbStandard"
      });
    },
    imageStyle: function() {
      return {
        backgroundImage: `url(https://static01.nyt.com/${this.largeImage.url})`,
        backgroundPosition: 'center',
        backgroundRepeat: 'no-repeat',
        width: '400px',
        height: '286px'
      }
    }
  },
  updated: function() {
    // console.log('updating: ', this.article.headline.main)
  },
  created: function () {
    // console.log('created', this.article.headline.main)
  }
}
</script>

<style lang="scss">
  .article {
    width: 50vw;
    height: 90vh;
    box-sizing: border-box;
    padding: 30px 0;
    .headline {
      .image {
        margin-bottom: 16px;
        min-width: 100%;
      }
      border-bottom: 1px solid #e2e2e2;
      padding-bottom: 12px;
      width: 75%;
      margin: 0 auto 6px;
      text-align: left;
      font-size: 1.5625rem;
      line-height: 1.6875rem;
      font-weight: 700;
    }
    .byline {
      text-align: left;
      width: 75%;
      margin: 0 auto 16px;
      text-transform: uppercase;
      font-size: 12px;
      font-weight: 100;
    }
    .snippet {
      width: 75%;
      margin: 0 auto;
      text-align: left;
      font-family: Georgia, serif;
      span.text::after {
        content: " "
      }
    }
  }

</style>
