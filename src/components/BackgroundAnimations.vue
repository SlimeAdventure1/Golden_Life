<script>
import BlobSnowflakes from "@/components/BlobSnowflakes";
import TachyonParticles from "@/components/tabs/time-dilation/TachyonParticles";
import CelestialAnimations from "@/components/CelestialAnimations";

export default {
  name: "BackgroundAnimations",
  components: {
    BlobSnowflakes,
    TachyonParticles,
    CelestialAnimations
  },
  data() {
    return {
      blob: false,
      animateTachyons: false,
      animateCelestials: false,
    };
  },
  methods: {
    update() {
      this.blob = Theme.currentName() === "S11";
      this.animateTachyons = player.options.animations.tachyonParticles &&
        Tabs.current[this.$viewModel.subtab].name === "Time Dilation";
      this.animateCelestials = player.options.animations.celreality
    }
  },
};
</script>

<template>
  <div
    id="ui-background-animations"
    class="l-background-animations"
  >
    <BlobSnowflakes v-if="blob" />
    <TachyonParticles v-if="animateTachyons" />
    <CelestialAnimations v-if="animateCelestials" />
  </div>
</template>

<style scoped>

</style>