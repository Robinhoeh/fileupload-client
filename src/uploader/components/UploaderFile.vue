<template>
  <div
    class="tw-bg-white tw-rounded-lg tw-p-4 tw-flex tw-items-stretch tw-mb-1"
  >
    <div>Progress</div>
    <div class="tw-flex tw-flex-col tw-justify-between">
      <div class="tw-mb-2">
        <div
          class="tw-font-medium tw-mr-3 tw-text-gray-700 tw-leading-tight"
        ></div>
        {{ upload.file.name }}
      </div>
      <div class="tw-text-gray-600 tw-text-sm tw-leading-tight">
        {{ sizeDisplay }} MB
      </div>

      <div class="tw-text-gray-600 tw-text-sm tw-align-baseline">
        <template v-if="state === states.WAITING">Waiting</template>
        <template v-if="state === states.UNSUPPORTED"
          >Sorry, this file type is unsupported</template
        >
        <template v-if="state === states.UPLOADING">Uploading</template>
      </div>
    </div>
  </div>
</template>

<script>
import states from "@/uploader/states";
import axios from "axios";

export default {
  data() {
    return {
      state: null,
      states,
    };
  },
  props: {
    upload: {
      type: Object,
      required: true,
    },
    baseURL: {
      type: String,
      required: true,
    },
    endpoint: {
      required: true,
      //Removed the type check
      //TODO:  add some sort of user feedback for non supported files
    },
  },
  computed: {
    sizeDisplay() {
      return (this.upload.file.size / 10000000).toFixed(2);
    },
  },
  methods: {
    makeFormData(file) {
      //prep data
      const form = new FormData();
      form.append("file", file, file.name);

      console.log(form);
      return form;
    },
    startUpload() {
      this.state = states.UPLOADING;

      axios.post(this.endpoint, this.makeFormData(this.upload.file), {
        baseURL: this.baseURL,
      });
    },
  },
  mounted() {
    if (this.endpoint === null) {
      return (this.state = states.UNSUPPORTED);
    }
    this.state = states.WAITING;

    this.startUpload();
  },
};
</script>

<style lang="scss" scoped></style>
