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
      flippedIndex: 999
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
      console.log(newState, oldState)
      if (newState === true) {
        setTimeout(() => {
          this.flippedIndex = this.index + 1
          let indexViewed = this.index + 2
          this.$emit('pageFlip', indexViewed)
          console.log("flipped: ", this.flippedIndex, this.index)

        }, 600)
      } else if (newState === false) {
          this.flippedIndex = 999 - this.index
          this.$emit('pageFlip', this.index)
          console.log("unflipped: ", this.flippedIndex, this.index)
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
    console.log("page created: ", this.articles)
    this.flippedIndex = 999 - this.index
  }
}
</script>

<style lang="scss">
  .flip-container {
    position:absolute;
    right: 0;
    top: left;
    perspective: 100vw;
    perspective-origin: left top;
    .flipper {
      transform-origin: 0% 0%;
    }
  }
  .flip-container.flip .flipper {
    transform: rotateY(-180deg);
    transform-origin: 0% 0%;


  }

  .flip-container, .front, .back {
    width: 50vw;
    // height: 480px;
  }

  .flipper {
    transition: 0.6s;
    transform-style: preserve-3d;
    position: relative;
  }

  .front, .back {
    backface-visibility: hidden;
    position: absolute;
    background-color: white;
    height: 90vh;
    // top: 0;
    // left: 0;
  }

  .front {
    /* for firefox 31 */
    transform: rotateY(0deg);
  }

  .back {
    transform: rotateY(180deg);
  }
</style>
