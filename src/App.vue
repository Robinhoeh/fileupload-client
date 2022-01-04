<template>
  <div class="container" id="app">
    <Uploader
      :options="{
        baseURL: 'http://vue-uploader.test:8000/api',
      }"
      :handlers="{
        // The format of the format comes from the file Object
        // the endpoint set below has to correspond with the POST endpoint
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
