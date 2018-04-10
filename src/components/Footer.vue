<template lang="html">
  <div class="footer">
    <div class="credit">
      Created by <a href="http://www.jasontseng.com">Jason Tseng</a>. Data courtesy of <a href="https://developer.nytimes.com/">The New York Times</a>
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
    padding: 5px 0;
    font-size: .8rem;
  }

  @media (min-width: 450px) {
    .footer {
      font-size: 1rem;
    }
  }
</style>
