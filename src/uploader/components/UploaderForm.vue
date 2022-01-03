<template>
  <form
    enctype="multipart/form-data"
    novalidate
    class="tw-bg-gray-200 tw-border-gray-400 tw-border-2 tw-border-dashed tw-rounded-lg tw-p-10 tw-flex tw-items-center tw-justify-center tw-mb-6 tw-relative"
  >
    <input
      @change="handleFileChosen"
      type="file"
      multiple
      class="tw-absolute tw-opacity-0 tw-w-full tw-h-full tw-top-0 tw-left-0"
    />
    <div class="tw-text-gray-700">
      Drop her to upload or <span class="tw-text-blue-500">Choose files</span>
    </div>
  </form>
</template>

<script>
export default {
  methods: {
    handleFileChosen(e) {
      this.$emit("chosen", e.target.files);
    },
  },
};
</script>

<style lang="scss" scoped></style>
