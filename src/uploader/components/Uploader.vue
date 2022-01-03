<template>
  <div>
    <UploaderForm @chosen="handleFilesChosen" />
    <UploaderFile
      v-for="(upload, i) in uploads"
      :key="i"
      endpoint="abc"
      :upload="upload"
      :baseURL="options.baseURL"
    />
  </div>
</template>

<script>
import UploaderForm from "./UploaderForm";
import UploaderFile from "./UploaderFile";
import options from "@/uploader/options";

export default {
  components: {
    UploaderForm,
    UploaderFile,
  },
  props: {
    options: {
      required: false,
      type: Object,
      default: () => options,
    },
    endpoint: {
      required: true,
      type: Object,
    },
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
