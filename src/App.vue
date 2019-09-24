<template>
  <div id="app">
    <image-slider :slides="[
      {
        image: 'https://i.imgur.com/Ix3HRcK.jpg',
        caption: 'Woof I\'m a doggo',
      },
      {
        image: 'https://i.imgur.com/ULMBt5g.jpg',
        caption: 'such wow',
      }
    ]"></image-slider>

    <input type="number" v-model="integer">
    <animated-integer :value="integer"></animated-integer>
  </div>
</template>

<script>
import ImageSlider from "../components/ImageSlider";
import AnimatedInteger from "../components/AnimatedInteger";

export default {
  name: 'app',
  components: {
    ImageSlider,
    AnimatedInteger,
  },
  data() {
    return {
      integer: 0,
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
