<template lang="html">
  <div class="page">
    <div class="flip-container" :style="{zIndex: this.flippedIndex}" :class="{flip: flip}">
      <div class="flipper">
        <div class="front" @click="this.flipCard">
          <articleCard class="bottom" :article="this.front" />
        </div>
        <div class="back" @click="this.flipCard">
          <articleCard class="top" :article="this.back" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ArticleCard from './Article'

export default {
  components: {
      ArticleCard
    },
  props: ['index', 'articles', 'resetPages'],
  data: function() {
    return {
      flip: false,
      flippedIndex: 999,
    }
  },
  computed: {
    front: function () {
      return this.articles[this.index]
    },
    back: function() {
      return this.articles[this.index + 1]
    }
  },
  watch: {
    flip: function (newState, oldState) {
      if (newState === true) {
        setTimeout(() => {
          this.flippedIndex = this.index + 1
          let indexViewed = this.index + 2
          this.$emit('pageFlip', indexViewed)

        }, 600)
      } else if (newState === false) {
          this.flippedIndex = 999 - this.index
          this.$emit('pageFlip', this.index)
      }
    },
    resetPages: function (newState, oldState) {
      if (newState === true) {
        this.flip = false;
      }
    }
  },
  methods: {
    flipCard: function() {
      this.flip = !this.flip;
    },
  },
  created: function () {
    this.flippedIndex = 999 - this.index
  }
}
</script>

<style lang="scss">
  @import "../assets/settings.scss";

  .flip-container {
    position:absolute;
    bottom: 0;
    perspective: 100vw;
    perspective-origin: center top;
    .flipper {
      transform-origin: 0% 50%;
    }
  }
  .flip-container.flip .flipper {
    transform: rotateX(180deg);
    transform-origin: 100% 50%;
  }

  .flip-container, .front, .back {
    height: 50%;
  }

  .flipper {
    transition: 0.6s;
    transform-style: preserve-3d;
    transform-origin: 100% 50%;

    position: relative;
  }

  .front, .back {
    backface-visibility: hidden;
    position: absolute;
    background-color: white;
  }

  .front {
    /* for firefox 31 */
    // transform: rotateX(0deg);
  }

  .back {
    transform: rotate3D(0, 1, 0, 180deg);
    transform-origin: center;
    .article.top {
      transform: rotate(180deg);
    }
  }

  @media (min-width: 450px) {
    .flip-container {
      position:absolute;
      right: 0;
      top: 0;
      perspective: 100vw;
      perspective-origin: left top;
      .flipper {
        transform-origin: 0% 50%;
      }
    }
    .flip-container, .front, .back {
      width: 50vw;
    }
    .flip-container.flip .flipper {
      transform: rotateY(-180deg);
      transform-origin: 0% 0%;
    }
    .front, .back {
      backface-visibility: hidden;
      position: absolute;
      background-color: white;
      height: $article_height;
    }
    .front {
      /* for firefox 31 */
      transform: rotateY(0deg);
    }
    .back {
      transform: rotateY(180deg);
      .article.top {
        transform: rotate(0deg);
      }
    }
  }
</style>
