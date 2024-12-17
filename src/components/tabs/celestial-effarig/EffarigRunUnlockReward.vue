<script>
export default {
  name: "EffarigRunUnlockReward",
  props: {
    unlock: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      isUnlocked: false
    };
  },
  computed: {
    descriptionLines() {
      return this.unlock.config.description.split("\n").map(x => x.trim());
    },
    isDoomed: () => Pelle.isDoomed,
    symbol: () => !Pelle.isDoomed ? GLYPH_SYMBOLS.effarig : Pelle.symbol,
  },
  methods: {
    update() {
      this.isUnlocked = this.unlock.isUnlocked;
    }
  }
};
</script>

<template>
  <div class="l-effarig-tab__reward o-descriptionBlock">
    <div class="c-effarig-tab__reward-label">
      {{ unlock.config.label }}
    </div>
    <div
      v-if="isUnlocked"
      class="l-effarig-tab__reward-descriptions"
    >
      <div
        v-for="(description, descriptionKey) in descriptionLines"
        :key="descriptionKey + '-effarig-reward-description'"
        class="c-effarig-tab__reward-description"
      >
        <span class="c-effarig-tab__reward-symbol">
          {{ symbol }}
        </span>
        <span :class="{ 'o-pelle-disabled': isDoomed }">
          {{ description }}
        </span>
      </div>
    </div>
    <span
      v-else
      class="c-effarig-tab__reward-symbol"
    >???</span>
  </div>
</template>
<style scoped>
.c-effarig-tab__reward-label{
  border-top:none
}
.o-descriptionBlock{
  padding: 0 0.5rem 0.5rem;
  margin-bottom: 0.3rem;
}
</style>