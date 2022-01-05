<template>
  <div>
    <UploaderForm @chosen="handleFilesChosen" />
    <div
      class="tw-mb-4 tw-flex tw-justify-between tw-px-4 tw-text-gray-600 tw-text-sm"
    >
      <span
        >{{ this.uploads.length }} uploads ({{ currentUploadCount }} in progress
        / {{ currentUploadCount }} complete)</span
      >
      <span> {{ overallProgress }} % complete </span>
    </div>
    <UploaderFile
      v-for="(upload, i) in uploads"
      :key="i"
      :endpoint="determineEndpointFor(upload.file.type)"
      :upload="upload"
      :baseURL="options.baseURL"
      @progress="handleUploadProgress"
      @change="handleUploadChange"
    />
  </div>
</template>

<script>
import UploaderForm from "./UploaderForm";
import UploaderFile from "./UploaderFile";
import options from "@/uploader/options";
import states from "@/uploader/states";
import get from "lodash.get";
import uuid from "uuid/v4";

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
    handlers: {
      required: true,
      type: Object,
    },
  },
  data() {
    return {
      uploads: [],
    };
  },
  computed: {
    // can these be more DRY?
    currentUploadCount() {
      return this.uploads.filter((upload) => upload.uploading).length;
    },
    completedUploadCount() {
      return this.uploads.filter((upload) => upload.complete).length;
    },
    overallProgress() {
      if (this.uploads.length === 0) return 0;

      let uploads = this.uploads.filter(
        (upload) => !upload.cancelled && !upload.failed
      );

      if (uploads.length === 0) return 0;

      return parseInt(
        uploads.reduce((a, b) => a + b.progress, 0) / uploads.length
      );
    },
  },
  methods: {
    determineEndpointFor(fileType) {
      return get(this.handlers[fileType], "endpoint", null);
    },
    handleFilesChosen(files) {
      this.uploads.push(
        ...Array.from(files).map((file) => {
          return {
            // Show the overall progress of all uploads without mutating the prop in
            // add uinque id to each induvidual upload
            id: uuid(),
            progress: 0,
            uploading: false,
            complete: false,
            cancelled: false,
            failed: false,
            file,
          };
        })
      );
    },
    handleUploadProgress(e) {
      this.uploads = this.uploads.map((upload) => {
        if (e.id === upload.id) {
          // re-sets the custom object progress property
          upload.progress = e.progress;
        }
        return upload;
      });
    },
    handleUploadChange(e) {
      switch (e.state) {
        case states.UPLOADING:
          this.uploads = this.uploads.map((upload) => {
            if (e.id === upload.id) {
              upload.uploading = true;
            }
            return upload;
          });
          break;
        case states.FAILED:
          this.uploads = this.uploads.map((upload) => {
            if (e.id === upload.id) {
              upload.failed = true;
              upload.uploading = false;
            }
            return upload;
          });
          break;
        case states.COMPLETE:
          this.uploads = this.uploads.map((upload) => {
            if (e.id === upload.id) {
              upload.complete = true;
              upload.uploading = false;
            }
            return upload;
          });
          break;
        case states.CANCELLED:
          this.uploads = this.uploads.map((upload) => {
            if (e.id === upload.id) {
              upload.cancelled = true;
              this.uploading = false;
            }
            return upload;
          });
          break;
      }
    },
  },
};
</script>

<style></style>
