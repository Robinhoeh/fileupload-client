<template>
  <div
    class="tw-bg-white tw-rounded-lg tw-p-4 tw-flex tw-items-stretch tw-mb-1"
  >
    <div><UploaderProgress :progress="progress" /></div>
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
        <template v-if="state === states.COMPLETE">Complete</template>
        <template v-if="state === states.FAILED">Failed</template>
        <template v-if="state === states.UNSUPPORTED"
          >Sorry, this file type is unsupported</template
        >
        <a href="" class="tw-text-blue-500" @click.prevent="handleCancel"
          >Cancel</a
        >
        <template v-if="state === states.UPLOADING">Uploading</template>
      </div>
    </div>
  </div>
</template>

<script>
// Update this structure - this component is now dependant on another component :(
import UploaderProgress from "./UploaderProgress";
import states from "@/uploader/states";
import axios from "axios";

export default {
  components: {
    UploaderProgress,
  },
  data() {
    return {
      state: null,
      progress: 0,
      axios: {
        cancel: null,
      },
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
  watch: {
    "upload.queuded"(queuded) {
      if (this.state === states.UNSUPPORTED) {
        return;
      }
      if (queuded === false) {
        this.startUpload();
      }
    },
    progress(progress) {
      this.$emit("progress", {
        id: this.upload.id,
        progress,
      });
    },
    state(state) {
      this.$emit("change", {
        id: this.upload.id,
        state,
      });
      switch (state) {
        case states.CANCELLED:
        case states.FAILED:
          this.progress = 0;
          break;
      }
    },
  },
  methods: {
    handleCancel() {
      this.axios.cancel();
    },
    makeFormData(file) {
      //prep data
      const form = new FormData();
      form.append("file", file, file.name);

      console.log(form);
      return form;
    },
    handleUploadProgress(e) {
      this.progress = Math.round((e.loaded * 100) / e.total);
    },
    startUpload() {
      this.state = states.UPLOADING;

      axios
        .post(this.endpoint, this.makeFormData(this.upload.file), {
          baseURL: this.baseURL,
          onUploadProgress: this.handleUploadProgress,
          //Cancel token axios
          cancelToken: new axios.CancelToken((token) => {
            this.axios.cancel = token;
          }),
        })
        .then(() => {
          this.state = states.COMPLETE;
        })
        .catch((e) => {
          console.error(e);
          // Check for existence of Cancel Object from axios
          if (e instanceof axios.Cancel) {
            return (this.state = states.CANCELLED);
          }

          this.state = states.FAILED;
        });
    },
  },
  mounted() {
    if (this.endpoint === null) {
      return (this.state = states.UNSUPPORTED);
    }
    this.state = states.WAITING;
  },
};
</script>

<style lang="scss" scoped></style>
