<template lang="html">
  <div class="modal" @click="handleClick" :class="{hide: clicked, low: transitionDone}" >
    <div class="instructions" @click="handleClick">
      Click either side to flip pages for more articles.
    </div>
  </div>
</template>

<script>
export default {
  data: function(){
    return {
      clicked: false,
      transitionDone: false,
    }
  },
  methods: {
    handleClick: function() {
      this.clicked = true;
      setTimeout(() => {
        this.transitionDone = true;
      }, 600);
    }
  }
}
</script>

<style lang="scss">
  .modal {
    background-color: rgba(255, 255, 255, .7);
    z-index: 99999999;
    height: 100vh;
    width: 100vw;
    position: fixed;
    transition: opacity .6s;
    &.hide {
      opacity: 0;
      &.low {
        display: none;
      }
    }
    .instructions {
      position: fixed;
      box-shadow: inset 0px 0px 200px 102px white, 0px 0px 30px 20px white;
      text-align: center;
      filter: drop-shadow(0px 0px 12px white);
      font-size: 1.3rem;
      width: 15rem;
      top: calc(50vh - 8px);
      left: calc(50vw - 7.5rem);
      z-index: 99999999;
    }
  }


  @media (min-width: 450px) {
    .modal {
      .instructions {
        font-size: 2rem;
        left: calc(50vw - 15rem);
        width: 30rem;
      }
    }
  }
</style>
