<template>
  <div class="tw-h-20 tw-w-20 tw-mr-6 tw-relative">
    <svg viewBox="0 0 120 120">
      <circle
        cx="60"
        cy="60"
        r="54"
        fill="none"
        stroke-width="12"
        class="tw-stroke-current tw-text-gray-200"
      />
    </svg>
    {{ progress }}
  </div>
</template>

<script>
export default {
  props: {
    progress: {
      required: true,
      type: Number,
    },
  },
};
</script>

<style lang="scss" scoped></style>
