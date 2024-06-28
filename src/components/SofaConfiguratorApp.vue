<template>
  <div
    id="sofa-configurator-app"
    v-cloak
  >
    <div class="sca__selected">
      <dl>
        <dt>Selected Pallet</dt>
        <dd>{{ selectedPallet.longName }}</dd>
      </dl>
      <dl>
        <dt>Selected Mattress</dt>
        <dd>{{ selectedMattress.longName }}</dd>
      </dl>
      <dl>
        <dt>Selected Pillow</dt>
        <dd>{{ selectedPillow.longName }}</dd>
      </dl>
    </div>
    <div class="sca__preview">
      <div class="sca__image-container">
        <VueImgTest />
        <img
          v-if="selectedPallet && selectedPallet.class !== 'none'"
          :src="imageSource"
          alt="Selected Pallet Image"
          class="sca__preview-image"
          width="1072"
          height="712"
        />
        <img
          v-if="selectedMattress && selectedMattress.class !== 'none'"
          :src="mattressImageSource"
          alt="Selected Mattress Image"
          class="sca__preview-image"
          width="1072"
          height="712"
        />
        <img
          v-if="selectedPillow && selectedPillow.class !== 'none'"
          :src="pillowImageSource"
          alt="Selected Pillow Image"
          class="sca__preview-image"
          width="1072"
          height="712"
        />
      </div>
    </div>
    <div class="sca__controls">
      <!-- Slider Controls -->
      <div class="sca__controls__bar">
        <button
          class="button"
          @click="prevStep"
          :disabled="currentStep === 0"
        >Back</button>
        <span><strong>{{ currentStepName }}</strong> {{
          currentStep + 1 }}/{{ steps.length }}</span>
        <button
          class="button"
          @click="nextStep"
          :disabled="currentStep === steps.length - 1"
        >Next</button>
      </div>
      <div class="sca__controls-options">
        <button
          id="btn-print"
          title="click to Print"
          v-if="!fullscreen"
          class="button sca__button"
          @click="printConfig"
        >
          <Printer24 />
        </button>
        <button
          title="click to see fullscreen"
          class="button"
          v-if="!fullscreen"
          @click="toggleFullscreen"
        >
          <FitToScreen24 />
        </button>
        <button
          title="click to exit fullscreen"
          class="button"
          v-if="fullscreen"
          @click="exitFullscreen"
        >
          <ShrinkScreen24 />
        </button>
      </div>

      <!-- Slider Content -->
      <div
        class="slider"
        :style="{ transform: `translateX(-${(100 / steps.length) * currentStep}%)` }"
      >
        <div
          class="slide"
          style="width: 100%; overflow-x: auto"
        >
          <Part
            v-show="currentStep === 0"
            :parts="pallets"
            :selectedPart="selectedPallet"
            :handlePartSelection="handlePalletSelection"
          />
        </div>
        <div
          class="slide"
          style="width: 100%; overflow-x: auto"
        >
          <Part
            v-show="currentStep === 1"
            :parts="mattresses"
            :selectedPart="selectedMattress"
            :handlePartSelection="handleMattressSelection"
          />
        </div>
        <div
          class="slide"
          style="width: 100%; overflow-x: auto"
        >
          <Part
            v-show="currentStep === 2"
            :parts="pillows"
            :selectedPart="selectedPillow"
            :handlePartSelection="handlePillowSelection"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import VueImgTest from "./VueImgTest.vue";
import { Printer24 } from "@carbon/icons-vue";
import { FitToScreen24 } from "@carbon/icons-vue";
import { ShrinkScreen24 } from "@carbon/icons-vue";
import Part from "./Part.vue";

export default {
  components: {
    VueImgTest,
    Part,
    Printer24,
    FitToScreen24,
    ShrinkScreen24,
  },
  data() {
    return {
      currentStep: 0,
      steps: ["Pallet", "Mattress", "Pillow"],
      // transitionName: 'slide-left',
      pallets: [
        { id: 1, value: "None", class: "none", hexCode: "#ccc", shortName: "None", longName: "No Color", image: "" },
        {
          id: 2,
          value: "Giverny",
          class: "giverny",
          hexCode: "#0088d5",
          shortName: "Giverny",
          longName: "Giverny Blue",
          image: "pallet-giverny.png",
        },
        {
          id: 3,
          value: "Pure",
          class: "pure",
          hexCode: "#e8e7e2",
          shortName: "Pure",
          longName: "Pure White",
          image: "pallet-pure.png",
        },
        {
          id: 4,
          value: "Graphite",
          class: "graphite",
          hexCode: "#393833",
          shortName: "Graphite",
          longName: "Graphite Gray",
          image: "pallet-graphite.png",
        },
        {
          id: 5,
          value: "Florence",
          class: "florence",
          hexCode: "#229682",
          shortName: "Florence",
          longName: "Florence Green",
          image: "pallet-florence.png",
        },
        {
          id: 6,
          value: "Barcelona Orange",
          class: "barcelona-orange",
          hexCode: "#de7932",
          shortName: "Orange",
          longName: "Barcelona Orange",
          image: "pallet-barcelona-orange.png",
        },
        {
          id: 7,
          value: "Arles Yellow",
          class: "arles-yellow",
          hexCode: "#d99c20",
          shortName: "Yellow",
          longName: "Arles Yellow",
          image: "pallet-arles-yellow.png",
        },
        {
          id: 8,
          value: "Provence",
          class: "provence",
          hexCode: "#55a9b9",
          shortName: "Provence",
          longName: "Provence Blue",
          image: "pallet-provence.png",
        },
        {
          id: 9,
          value: "English Yellow",
          class: "english-yellow",
          hexCode: "#d4bb3c",
          shortName: "Yellow",
          longName: "English Yellow",
          image: "pallet-english-yellow.png",
        },
        {
          id: 10,
          value: "Antibe Green",
          class: "antibe-green",
          hexCode: "#5bb842",
          shortName: "Antibe",
          longName: "Antibe Green",
          image: "pallet-antibe-green.png",
        },
        {
          id: 11,
          value: "Emperor Silk",
          class: "emperor-silk",
          hexCode: "#bd3641",
          shortName: "Red",
          longName: "Emperor Silk",
          image: "pallet-emperor-silk.png",
        },
        {
          id: 12,
          value: "Scandinavian Pink",
          class: "scandinavian-pink",
          hexCode: "#d5887f",
          shortName: "Pink",
          longName: "Scandinavian Pink",
          image: "pallet-scandinavian-pink.png",
        },
      ],
      selectedPallet: {
        value: "None",
        class: "none",
        hexCode: "#ccc",
        shortName: "None",
        longName: "No Color",
        image: "",
      },
      mattresses: [
        {
          id: 1,
          value: "None",
          class: "none",
          hexCode: "#ccc",
          shortName: "None",
          longName: "No Mattress fabric",
          image: "",
        },
        {
          id: 2,
          value: "Blue",
          class: "blue",
          hexCode: "#0000ff",
          shortName: "Blue",
          longName: "Blue Mattress",
          image: "mattress-blue.png",
        },
        {
          id: 3,
          value: "Green",
          class: "green",
          hexCode: "#00ff00",
          shortName: "Green",
          longName: "Green Mattress",
          image: "mattress-green.png",
        },
        {
          id: 4,
          value: "Orange",
          class: "orange",
          hexCode: "#ff9900",
          shortName: "Orange",
          longName: "Orange Mattress",
          image: "mattress-orange.png",
        },
        {
          id: 5,
          value: "Pink",
          class: "pink",
          hexCode: "#ff69b4",
          shortName: "Pink",
          longName: "Pink Mattress",
          image: "mattress-pink.png",
        },
        {
          id: 6,
          value: "Red",
          class: "red",
          hexCode: "#ff0000",
          shortName: "Red",
          longName: "Red Mattress",
          image: "mattress-red.png",
        },
      ],
      selectedMattress: {
        value: "None",
        class: "none",
        hexCode: "#ccc",
        shortName: "None",
        longName: "No Mattress",
        image: "",
      },
      pillows: [
        {
          id: 1,
          value: "None",
          class: "none",
          hexCode: "#ccc",
          shortName: "None",
          longName: "No Pillow fabric",
          image: "",
        },
        {
          id: 2,
          value: "Blue",
          class: "blue",
          hexCode: "#0000ff",
          shortName: "Blue",
          longName: "Blue Pillows",
          image: "pillows-blue.png",
        },
        {
          id: 3,
          value: "Brown",
          class: "brown",
          hexCode: "#8b4513",
          shortName: "Brown",
          longName: "Brown Pillows",
          image: "pillows-brown.png",
        },
        {
          id: 4,
          value: "Green",
          class: "green",
          hexCode: "#00ff00",
          shortName: "Green",
          longName: "Green Pillows",
          image: "pillows-green.png",
        },
        {
          id: 5,
          value: "Orange",
          class: "orange",
          hexCode: "#ff9900",
          shortName: "Orange",
          longName: "Orange Pillows",
          image: "pillows-orange.png",
        },
        {
          id: 6,
          value: "Pink",
          class: "pink",
          hexCode: "#ff69b4",
          shortName: "Pink",
          longName: "Pink Pillows",
          image: "pillows-pink.png",
        },
        {
          id: 7,
          value: "Red",
          class: "red",
          hexCode: "#ff0000",
          shortName: "Red",
          longName: "Red Pillows",
          image: "pillows-red.png",
        },
      ],
      selectedPillow: {
        value: "None",
        class: "none",
        hexCode: "#ccc",
        shortName: "None",
        longName: "No Pillow",
        image: "",
      },
      fullscreen: false,
    };
  },
  computed: {
    currentStepName() {
      return this.steps[this.currentStep];
    },
    imageSource() {
      const { selectedPallet } = this;

      if (!selectedPallet) {
        return "";
      }

      // Assuming your selectedPallet has an 'image' property
      const palletImage = selectedPallet.image;
      return `./images/sofa-cf/${palletImage}`;
    },
    mattressImageSource() {
      const { selectedMattress } = this;

      if (!selectedMattress) {
        return "";
      }

      const mattressImage = selectedMattress.image;
      return `./images/sofa-cf/${mattressImage}`;
    },
    pillowImageSource() {
      const { selectedPillow } = this;

      if (!selectedPillow) {
        return "";
      }

      const pillowImage = selectedPillow.image;
      return `./images/sofa-cf/${pillowImage}`;
    },
  },
  methods: {
    handleImageLoad() {
      console.log("Image loaded successfully");
    },
    handleImageError() {
      console.error("Error loading image");
    },
    nextStep() {
      if (this.currentStep < this.steps.length - 1) {
        this.currentStep++;
      }
    },
    prevStep() {
      if (this.currentStep > 0) {
        this.currentStep--;
      }
    },
    updateSelectedPallet(pallet) {
      this.selectedPallet = pallet;
    },
    setInitialPalletFromQuery() {
      const urlSearchParams = new URLSearchParams(window.location.search);
      const palletFromQuery = urlSearchParams.get("pallet");
      // Find the pallet with the matching value in the URL parameter
      this.selectedPallet =
        this.pallets.find((pallet) => pallet.value.toLowerCase() === palletFromQuery) ||
        this.pallets.find((pallet) => pallet.value.toLowerCase() === "none");
    },
    handlePalletSelection(pallet) {
      this.updateSelectedPallet(pallet);

      // Update the URL without triggering a page reload
      const urlSearchParams = new URLSearchParams(window.location.search);
      urlSearchParams.set("pallet", pallet.value.toLowerCase());
      window.history.replaceState({}, "", `${window.location.pathname}?${urlSearchParams}`);
    },
    updateSelectedMattress(mattress) {
      this.selectedMattress = mattress;
    },
    handleMattressSelection(mattress) {
      this.updateSelectedMattress(mattress);
      // Update the URL without triggering a page reload
      const urlSearchParams = new URLSearchParams(window.location.search);
      urlSearchParams.set("mattress", mattress.value.toLowerCase());
      window.history.replaceState({}, "", `${window.location.pathname}?${urlSearchParams}`);
    },
    setInitialMattressFromQuery() {
      const urlSearchParams = new URLSearchParams(window.location.search);
      const mattressFromQuery = urlSearchParams.get("mattress");
      this.selectedMattress =
        this.mattresses.find((mattress) => mattress.value.toLowerCase() === mattressFromQuery) ||
        this.mattresses.find((mattress) => mattress.value.toLowerCase() === "none");
    },
    updateSelectedPillow(pillow) {
      this.selectedPillow = pillow;
    },

    handlePillowSelection(pillow) {
      this.updateSelectedPillow(pillow);

      // Update the URL without triggering a page reload
      const urlSearchParams = new URLSearchParams(window.location.search);
      urlSearchParams.set("pillow", pillow.value.toLowerCase());
      window.history.replaceState({}, "", `${window.location.pathname}?${urlSearchParams}`);
    },

    setInitialPillowFromQuery() {
      const urlSearchParams = new URLSearchParams(window.location.search);
      const pillowFromQuery = urlSearchParams.get("pillow");
      this.selectedPillow =
        this.pillows.find((pillow) => pillow.value.toLowerCase() === pillowFromQuery) ||
        this.pillows.find((pillow) => pillow.value.toLowerCase() === "none");
    },
    toggleFullscreen() {
      const appContainer = document.getElementById("sofa-configurator-app");

      // Toggle fullscreen mode for the app container
      this.fullscreen = !this.fullscreen;

      if (this.fullscreen) {
        appContainer.requestFullscreen();
      } else {
        document.exitFullscreen();
      }
    },
    exitFullscreen() {
      // Exit fullscreen mode for the app container
      this.fullscreen = false;
      document.exitFullscreen();
    },
    // Add selectMattress and selectPillow methods
    printConfig() {
      window.print();
    },
  },
  created() {
    console.log("created");
  },
  mounted() {
    console.log("mounted");
    // Set initial pallet from query parameter on component mount
    this.setInitialPalletFromQuery();
    this.setInitialMattressFromQuery();
    this.setInitialPillowFromQuery();
  },
};
</script>

<style lang="css" scoped>
[v-cloak] {
  display: none;
}

#sofa-configurator-app {
  display: grid;
  /* grid-template-rows: 10svh 50svh 20svh; */
  grid-template-rows: auto 1fr auto;
  align-content: space-between;
  border-radius: 12px;
  position: relative;
  padding: 1rem;
  border: 1px solid;
  margin: 1rem;
  font-family: -apple-system, blinkmacsystemfont, "Segoe UI", roboto, "Helvetica Neue", arial, sans-serif;
  height: calc(100svh - 2rem);
}

.sca__selected {
  display: flex;
  flex-direction: column;
  gap: 8px;
  flex-wrap: wrap;
  height: min-content;
}

@media only screen and (min-width: 768px) {
  .sca__selected {
    flex-direction: row;
  }
}

.sca__selected dl {
  display: flex;
  gap: 8px;
  flex: 1;
}

.sca__selected dl,
.sca__selected dd {
  margin: 0;
}

.sca__selected dt {
  display: none;
}

.sca__selected dt,
.sca__selected dd {
  white-space: nowrap;
}

.sca__image-container {
  position: relative;
  height: 100%;
  display: grid;
  /* place-content: center; */
  justify-items: center;
  align-items: center;
}

.sca__controls {
  overflow: hidden;
  display: flex;
  flex-direction: column;
  /* gap: 32px; */
  width: 100%;
  /* justify-content: flex-end; */
}

.sca__preview {
  border-radius: 12px;
  /* background-color: #f6f6f6; */
}

.sca__preview-image {
  display: block;
  width: auto;
  margin: 0 auto !important;
  max-height: 100%;
  grid-column: 1;
  grid-row: 1;
  height: auto;
  max-width: 100%;
  background-repeat: no-repeat;
  background-size: cover;
  font-style: italic;
  shape-margin: 1rem;
  position: absolute;
}

.sca__controls-options {
  position: absolute;
  top: 1rem;
  right: 1rem;
  display: flex;
  gap: 1rem;
}

@media only screen and (min-width: 768px) {
  .sca__controls-options {
    /* padding: 1.5rem 2rem; */
  }
}

@media only screen and (max-width: 767px) {}

#sofa-configurator-app:fullscreen .sca__controls-options {
  top: 2rem;
  right: 2rem;
}

.slider {
  display: flex;
  width: 300%;
  /* Adjust the width based on the number of steps */
  transition: transform 0.5s ease;
}

.sca__controls__bar {
  display: flex;
  align-items: center;
  /* justify-content: center; */
  /* gap: 3rem; */
  justify-content: space-between;
  margin: 0 auto;
  width: 100%;
}

@media only screen and (min-width: 768px) {
  .sca__controls__bar {
    width: max-content;
    gap: 3rem;
  }
}

.slide::-webkit-scrollbar {
  width: 12px;
  /* width of the scrollbar */
}

.slide::-webkit-scrollbar-track {
  background: #f1f1f1;
  /* color of the track */
}

.slide::-webkit-scrollbar-thumb {
  background: #ccc;
  /* color of the thumb */
  border-radius: 6px;
  /* rounded corners of the thumb */
}

.slide::-webkit-scrollbar-thumb:hover {
  background: #555;
  /* color of the thumb on hover */
}

#sofa-configurator-app button {
  font-family: inherit;
}

.button {
  padding: 0.5rem 1rem;
  background-color: transparent;
  border: 1px solid currentColor;
  border-radius: 4px;
  color: currentColor;
}

.button[disabled] {
  opacity: 0.3;
}

@media (pointer: coarse) {
  .sca__controls-options {
    display: none;
  }
}

@media print {

  .sca__controls,
  .sca__controls-options {
    display: none !important;
  }
}
</style>
