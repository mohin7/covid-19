<template>
  <div class="container">
    <div class="app">
      <!-- header start  -->
      <HeaderArea />
      <!-- header end -->

      <nuxt/>
    </div>
  </div>
</template>

<script>
import HeaderArea from '@/components/HeaderArea'
export default {
  components: {
    HeaderArea
  }
}
</script>

<style>
html {
  font-family: 'Source Sans Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}
body{
  background-color: #E9F1F9;
}
h1,h2,h3,h4,h5,h6{
font-weight: 700;
}
*,
*:before,
*:after {
  box-sizing: border-box;
  margin: 0;
}
</style>
