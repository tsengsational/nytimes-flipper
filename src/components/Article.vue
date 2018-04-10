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
      if (window.innerWidth >= 450) {
        return {
          backgroundImage: `url(https://static01.nyt.com/${this.largeImage.url})`,
          backgroundPosition: 'center',
          backgroundRepeat: 'no-repeat',
          width: '400px',
          height: '286px'
        }
      } else {
        return {
          backgroundImage: `url(https://static01.nyt.com/${this.thumbImage.url})`,
          backgroundPosition: 'center',
          backgroundRepeat: 'no-repeat',
          width: '75px',
          height: '75px'
        }
      }
    }
  }
}
</script>

<style lang="scss">
  .article {
    background-color: white;
    width: 100vw;
    height: 43vh;
    box-sizing: border-box;
    padding: 30px 0;
    .headline {
      .image {
        display: inline-block;
      }
      border-bottom: 1px solid #e2e2e2;
      margin: 0 auto 6px;
      width: 90%;
      text-align: left;
      font-size: 1.15rem;
      line-height: 1.25rem;
      font-weight: 700;
      span {
        display: inline-block;
        width: calc(90vw - 90px);
        margin-left: 10px;
        vertical-align: top;
      }
    }
    .byline {
      text-align: left;
      width: 90%;
      margin: 0 auto 16px;
      text-transform: uppercase;
      font-size: 12px;
      font-weight: 100;
    }
    .snippet {
      width: 90%;
      margin: 0 auto;
      text-align: left;
      font-size: 14px;
      font-family: Georgia, serif;
      span.text::after {
        content: " "
      }
    }
  }

  @media (min-width: 450px) {
    .article {
      width: 50vw;
      .headline {
        .image {
          min-width: 100%;
          margin-bottom: 16px;
        }
        width: 75%;
        padding-bottom: 12px;
        font-size: 1.5625rem;
        line-height: 1.6875rem;
        font-weight: 700;
        span {
          display: inline;
          width: 100%;
          margin-left: 0;
          position: inherit;
          top: 0;
        }
      }
      .byline {
        width: 75%;
      }
      .snippet {
        width: 75%;
        font-size: 16px;
      }
    }
  }

</style>
