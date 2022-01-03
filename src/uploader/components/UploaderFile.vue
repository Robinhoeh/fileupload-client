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
      <div class="tw-text-gray-600 tw-text-sm tw-leading-tight">mb size</div>

      <div class="tw-text-gray-600 tw-text-sm tw-align-baseline">Cancelled</div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    upload: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style lang="scss" scoped></style>
