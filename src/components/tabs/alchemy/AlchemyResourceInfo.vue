<script>
import EffectDisplay from "@/components/EffectDisplay";

export default {
  name: "AlchemyResourceInfo",
  components: {
    EffectDisplay
  },
  props: {
    resource: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      amount: 0,
      cap: 0,
      capped: false,
      flow: 0,
      isReactionActive: false,
      reactionProduction: 0,
      isUnlocked: false,
      unlockRequirement: ""
    };
  },
  computed: {
    classObject() {
      return {
        "c-alchemy-resource-info": true,
        "c-alchemy-resource-info--locked": !this.isUnlocked
      };
    },
    reaction() {
      return this.resource.reaction;
    },
    isBaseResource() {
      return this.resource.isBaseResource;
    },
    reactionText() {
      if (this.resource === AlchemyResource.reality) return this.realityReactionText;
      const reagents = this.reaction.reagents
        .map(r => `${format(r.cost)}${r.resource.symbol}`)
        .join(" + ");
      return `${reagents} ➜ ${format(this.reactionProduction, 2, 2)}${this.resource.symbol}`;
    },
    realityReactionText() {
      const reagents = this.reaction.reagents
        .map(r => r.resource.symbol)
        .join(" + ");
      return `${reagents} ➜ ${this.resource.symbol}`;
    },
    effectConfig() {
      const resource = this.resource;
      return {
        effect: () => resource.effectValue,
        formatEffect: resource.config.formatEffect
      };
    },
    resourceAmount() {
      return formatFloat(this.amount, 1);
    },
    resourceCap() {
      return formatFloat(this.cap, 1);
    },
    formattedFlow() {
      const sign = this.flow >= 0 ? "+" : "-";
      if (Math.abs(this.flow) < 0.01) return "None";
      const resourceText = `${sign}${format(Math.abs(this.flow), 2, 2)}/sec`;
      const color = this.flow > 0 ? "9CCC65" : "CC6666";
      return `<span style="color:#${color}">${resourceText}</span>`;
    },
    isDoomed: () => Pelle.isDoomed,
    Background(){
      return{
        background:`linear-gradient(135deg,${this.color} -25%,transparent 37.5%), var(--leg-base-render)`
      }
    },
    colorStyle(){
      return {
        background: this.isUnlocked?`radial-gradient(at top,white -50%,${this.color},black)`:`radial-gradient(at top,white -50%,#444444,black)`,
      };
    }
  },
  methods: {
    update() {
      const resource = this.resource;
      this.amount = resource.amount;
      this.color = resource._config.color;
      this.cap = resource.cap;
      this.capped = resource.capped;
      this.flow = resource.flow;
      this.isUnlocked = resource.isUnlocked;
      this.unlockRequirement = resource.lockText;
      if (!this.isBaseResource) {
        this.isReactionActive = !this.isDoomed && this.reaction.isActive;
        this.reactionProduction = this.reaction.reactionProduction;
      }
    }
  }
};
</script>

<template>
  <div
    v-if="isUnlocked"
    :class="classObject"
    :style="Background"
  >
  <div style="margin: -8rem -2rem 0rem;display: flex;position: absolute;">
    <div class="l-resource-component" :style="colorStyle"><div style="padding-bottom: 0.3rem;">{{ resource.symbol }}</div></div>
  </div>
    <span class="c-alchemy-resource-info__title">
      {{ resource.symbol }} <span style="font-family:cambria;font-size:1.75rem;line-height:1.2">{{ resource.name }}</span> {{ resource.symbol }}
    </span>
    <span v-if="isBaseResource">Base Resource</span>
    <span v-else>Reaction: {{ isReactionActive ? "Active" : "Inactive" }} ({{ reactionText }})</span>
    <span v-if="isDoomed">
      Destroyed by Pelle
    </span>
    <span v-else>
      {{ capped ? "Capped" : "Current" }}: {{ resourceAmount }} / {{ resourceCap }}
      (Recent change: <span v-html="formattedFlow" />)
    </span>
    <span :class="{ 'o-pelle-disabled': isDoomed }">
      <EffectDisplay
        label="Effect"
        :config="effectConfig"
      />
    </span>
  </div>
  <div
    v-else
    :class="classObject"
  >
  <div style="margin: -8rem -2rem 0rem;display: flex;position: absolute;">
    <div class="l-resource-component" :style="colorStyle"><div style="padding-top: 0.3rem;">?</div></div>
  </div>
    Unlock requirement: {{ unlockRequirement }}
  </div>
</template>

<style scoped>

</style>
