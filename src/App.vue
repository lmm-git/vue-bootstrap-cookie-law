<template>
  <div id="app">
    <cookie-law information-button-link="https://marschke.me" close-button-variant="success" theme="gray-transparent">
      <p slot="message">Write something very long long long long long long long long long long long long long long long...</p>
    </cookie-law>
  </div>
</template>

<script>
import CookieLaw from './components/CookieLaw'

export default {
  name: 'app',
  components: {
    CookieLaw
  }
}
</script>

<style lang="scss">
  @import '~bootstrap/scss/bootstrap';
</style>
