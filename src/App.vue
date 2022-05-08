<template>
  <section>
   <PhotoUploader v-model="files" />
  </section>
</template>

<script>
import { ref } from "vue";
import PhotoUploader from "@/components/PhotoUploader";

export default ({
  name: 'App',
  components: {
    PhotoUploader
  },
  setup () {
    const files = ref([])

    return{
      files
    }
  }
});
</script>

<style lang="scss">
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  body {
    margin: 0;
    padding: 100px 15px;
  }
  img {
    max-width: 100%;
    vertical-align: middle;
  }
</style>
