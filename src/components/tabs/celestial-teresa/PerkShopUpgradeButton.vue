<script>
import CostDisplay from "@/components/CostDisplay";
import DescriptionDisplay from "@/components/DescriptionDisplay";
import EffectDisplay from "@/components/EffectDisplay";
import HintText from "@/components/HintText";

export default {
  name: "PerkShopUpgradeButton",
  components: {
    DescriptionDisplay,
    EffectDisplay,
    CostDisplay,
    HintText
  },
  props: {
    upgrade: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      isAvailableForPurchase: false,
      isCapped: false,
      boughtAmount:0,
    };
  },
  computed: {
    isDoomed: () => Pelle.isDoomed,
    classObject() {
      return {
        "o-teresa-shop-button": true,
        "o-teresa-shop-button--available": this.isAvailableForPurchase && !this.isCapped,
        "o-teresa-shop-button--capped": this.isCapped,
        "o-teresa-shop-button--pelle-disabled": this.isDoomed &&
          (this.upgrade === PerkShopUpgrade.musicGlyph || this.upgrade === PerkShopUpgrade.fillMusicGlyph)
      };
    },
    LevelDisplay() {
      if (this.upgrade.id >= 4) return;
      return `Level ${this.boughtAmount}`
    }
  },
  methods: {
    update() {
      this.isAvailableForPurchase = this.upgrade.isAvailableForPurchase;
      this.isCapped = this.upgrade.isCapped;
      this.boughtAmount = this.upgrade.boughtAmount;
    }
  }
};
</script>

<template>
  <div class="l-spoon-btn-group">
    <button
      :class="classObject"
      @click="upgrade.purchase()"
    >
    <HintText
      type="perkShop"
      class="l-hint-text--perk-shop"
    >{{LevelDisplay}}</HintText>
      <DescriptionDisplay
        :config="upgrade.config"
        :length="70"
      />
      <br>
      <EffectDisplay :config="upgrade.config" />
      <br>
      <CostDisplay
        v-if="!isCapped"
        :config="upgrade.config"
        name="Perk Point"
      />
    </button>
  </div>
</template>

<style scoped>
.o-teresa-shop-button--pelle-disabled {
  text-decoration: line-through;
  cursor: auto;
}
</style>
