<template>
  <form
    @dragover.prevent="handleDragOver"
    @dragleave.prevent="handleDragLeave"
    enctype="multipart/form-data"
    novalidate
    class="tw-bg-gray-200 tw-border-gray-400 tw-border-2 tw-border-dashed tw-rounded-lg tw-p-10 tw-flex tw-items-center tw-justify-center tw-mb-6 tw-relative"
    :class="{'tw-border-gray-600': isDragging}"
  >
    <input
      @change="handleFilesChosen"
      type="file"
      multiple
      class="tw-absolute tw-opacity-0 tw-w-full tw-h-full tw-top-0 tw-left-0"
    />
    <template v-if="isDragging">
      <div class="tw-text-gray-700">
        Nearly there. Let go to upload
        <span class="tw-font-medium">{{ draggingCount }} items!</span>
      </div>
    </template>
    <template v-else>
      <div class="tw-text-gray-700">
        Drop her to upload or <span class="tw-text-blue-500">Choose files</span>
      </div>
    </template>
  </form>
</template>

<script>
export default {
  data() {
    return {
      isDragging: false,
      draggingCount: 0,
    };
  },
  methods: {
    handleFilesChosen(e) {
      this.$emit("chosen", e.target.files);
      this.isDragging = false;
    },
    handleDragOver(e) {
      this.isDragging = true;
      this.draggingCount = e.dataTransfer.items.length;
    },
    handleDragLeave() {
      this.isDragging = false;
      this.draggingCount = 0;
    },
  },
};
</script>

<style lang="scss" scoped></style>
