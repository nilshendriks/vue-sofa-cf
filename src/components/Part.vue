<template>
  <div class="part__options">
    <div
      class="part__option"
      @click="handlePartSelection(part)"
      v-for="part in parts"
      :key="part.value"
    >
      <span
        :title="part.longName"
        :class="['part__swatch', part.class, { 'selected': isSelected(part) }]"
        :style="{ backgroundColor: part.hexCode }"
      ></span>
      <p class="part__swatch-label">{{ part.shortName }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    parts: {
      type: Array,
      required: true,
    },
    selectedPart: {
      type: Object,
      required: true,
    },
    handlePartSelection: {
      type: Function,
      required: true,
    },
  },
  methods: {
    isSelected(part) {
      return this.selectedPart.class === part.class;
    },
  },
};
</script>

<style scoped>
.part__swatch {
  display: flex;
  width: 44px;
  height: 44px;
  border-radius: 100%;
}

.part__swatch.selected {
  outline: 1px solid #e8e8e8;
  outline-offset: 4px;
}

.part__swatch-label {
  visibility: hidden;
  font-family: var(--font-family-system, sans-serif);
  font-size: 14px;
  font-weight: 700;
  margin: 0;
  text-align: center;
}

.part__swatch.selected + .part__swatch-label {
  visibility: visible;
}

.part__options {
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  inline-size: fit-content;
  margin: 0 auto;
}

.part__option {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  width: 85px;
  flex-shrink: 0;
  padding-block: 1em;
}

</style>
