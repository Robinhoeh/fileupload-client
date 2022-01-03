<template>
  <div class="container" id="app">
    <Uploader></Uploader>
  </div>
</template>

<script>
import Uploader from "@/uploader/components/Uploader";

export default {
  name: "App",
  components: {Uploader},
};
</script>

<style src="@/assets/styles/app.css"></style>
