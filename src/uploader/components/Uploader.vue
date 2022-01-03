<template>
  <div>
    <UploaderForm @chosen="handleFilesChosen" />
    <UploaderFile v-for="(upload, i) in uploads" :key="i" :upload="upload" />
  </div>
</template>

<script>
import UploaderForm from "./UploaderForm";
import UploaderFile from "./UploaderFile";

export default {
  components: {
    UploaderForm,
    UploaderFile,
  },
  data() {
    return {
      uploads: [],
    };
  },
  methods: {
    handleFilesChosen(files) {
      console.log(files);
      this.uploads.push(
        ...Array.from(files).map((file) => {
          return {file};
        })
      );
    },
  },
};
</script>

<style></style>
