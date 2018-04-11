<template lang="html">
  <div class="footer">
    <div class="credit">
      <span class="created">Created by <a href="http://www.jasontseng.com">Jason Tseng</a>. </span>
      <a href="https://developer.nytimes.com/"><img src="http://static01.nytimes.com/packages/images/developer/logos/poweredby_nytimes_150a.png" class="nytimes-data" alt=""></a>
    </div>
  </div>
</template>

<script>
export default {
}
</script>

<style lang="scss">
  @import "../assets/settings.scss";

  .footer {
    border-top: $gray_border;
    font-size: .8rem;
    .credit {
      span.created {
        position: relative;
        top: -10px;
      }
      img.nytimes-data {
        max-height: 30px;
        top: 0;
      }
    }
  }

  @media (min-width: 450px) {
    .footer {
      font-size: 1rem;
    }
  }
</style>
