<template>
  <div class="container" id="app">
    <Uploader
      :options="{
        baseURL: 'hey.com',
      }"
      :endpoint="{
        'video/mp4': {
          endpoint: 'uploads/video',
        },
      }"
    />
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
