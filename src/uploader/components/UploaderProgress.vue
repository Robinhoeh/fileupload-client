<template>
  <div class="tw-h-20 tw-w-20 tw-mr-6 tw-relative">
    <div
      class="tw-absolute tw-inset-0 tw-flex tw-items-center tw-justify-center"
    >
      <span
        class="tw-text-blue-500 tw-text-sm tw-font-medium tw-leading-tight"
        >{{ progress }}</span
      >
    </div>
    <svg viewBox="0 0 120 120" class="tw--rotate-90">
      <circle
        cx="60"
        cy="60"
        :r="radius"
        fill="none"
        stroke-width="12"
        class="tw-stroke-current tw-text-gray-200"
      />
      <circle
        cx="60"
        cy="60"
        :r="radius"
        fill="none"
        stroke-width="12"
        class="tw-stroke-current tw-text-blue-500"
        :stroke-dasharray="dash"
        :stroke-dashoffset="offset"
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
  data() {
    return {
      radius: 54,
    };
  },
  computed: {
    dash() {
      return 2 * Math.PI * this.radius;
    },
    offset() {
      let circ = this.dash;
      let progress = this.progress / 100;

      return circ * (1 - progress);
    },
  },
};
</script>

<style lang="scss" scoped></style>
