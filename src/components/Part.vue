<template>
  <div class="part__options">
    <!-- <div id="mypopover" popover>Popover content</div> -->
    <div
      class="part__option"
      v-for="part in parts"
      :key="part.value"
    >
      <button
      :class="['part__swatch', part.class, { 'selected': isSelected(part) }]"
      :style="{ backgroundColor: part.hexCode }"
      @click="handlePartSelection(part)"
      :title="part.longName"
    >{{ part.shortName }}</button>
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
  /* display: flex; */
  width: 44px;
  height: 44px;
  border-radius: 100%;
  aspect-ratio: 1/1;
  /* text-indent: -9999em; */
  font-size: 0;
  border: 1px solid transparent;
  outline-offset: 4px;
  /* outline: 2px solid #ff0000;
   */
}

.part__swatch.selected {
  outline: 2px solid #ccc;
  outline-offset: 4px;
}

.part__swatch-label {
  /* visibility: hidden; */
  font-family: var(--font-family-system, sans-serif);
  font-size: 12px;
  font-weight: 400;
  margin: 0;
  text-align: center;
}

.part__swatch.selected + .part__swatch-label {
  /* visibility: visible; */
  font-weight: 700;
}

.part__options {
  display: flex;
  justify-content: flex-start;
  align-items: flex-start;
  inline-size: fit-content;
  margin: 0 auto;
  gap: 1em;
  padding: 1em 0;
}

.part__option {
  /* display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
  width: 85px;
  flex-shrink: 0;
  padding-block: 1em; */

  /* aspect-ratio: 1/1;
  text-indent: -9999em;
  border: 1px solid transparent; */

  display: flex;
  flex-direction: column;
  gap: 8px;
  align-items: center;
}

</style>
