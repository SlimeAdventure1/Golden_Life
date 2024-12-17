<script>
export default {
  name: "GameSpeedDisplay",
  props: {
  },
  data() {
    return {
      baseSpeed: 0,
      pulsedSpeed: 0,
      hasSeenAlteredSpeed: false,
      isStopped: false,
      isEC12: false,
      isPulsing: false,
      gametimeequal:0,
      fade:0,
    };
  },
  computed: {
    baseSpeedText() {
      if (this.isStopped) {
        return "Stopped (storing real time)";
      }
      const speed = this.formatNumber(this.baseSpeed);
      if (this.isEC12) {
        return `${speed} (fixed)`;
      }
      return `${speed}`;
    },
    pulseSpeedText() {
      return `${this.formatNumber(this.pulsedSpeed)}`;
    },
    baseText() {
      if (!this.hasSeenAlteredSpeed) return null;
      return this.baseSpeed === 1
        ? "The game is running at normal speed."
        : `Game speed is altered: ${this.baseSpeedText}`;
    },
    equalText(){
      if (this.baseSpeed !== 1) return `(${this.gametimeequal} / real second)`
    }
  },
  methods: {
    update() {
      this.baseSpeed = getGameSpeedupFactor();
      this.pulsedSpeed = getGameSpeedupForDisplay();
      this.hasSeenAlteredSpeed = PlayerProgress.seenAlteredSpeed();
      this.gametimeequal = TimeSpan.fromMilliseconds(getGameSpeedupForDisplay()*1000).toStringShort(false)
      this.isStopped = Enslaved.isStoringRealTime;
      this.isEC12 = EternityChallenge(12).isRunning;
      this.isPulsing = (this.baseSpeed !== this.pulsedSpeed) && Enslaved.canRelease(true);
      this.fade = GameEnd.endState>1&&GameEnd.endState<END_STATE_MARKERS.CREDITS_START? Math.clamp(1-(GameEnd.endState-3),0,1):1
    },
    formatNumber(num) {
      if (num >= 0.001 && num < 10000 && num !== 1) {
        return format(num, 3, 3);
      }
      if (num < 0.001) {
        return `${formatInt(1)} / ${format(1 / num, 2)}`;
      }
      return `${format(num, 2)}`;
    }
  }
};
</script>

<template>
  <span class="c-gamespeed" :style="{'opacity':fade}">
    <span>
      {{ baseText }}
    </span>
    <span v-if="isPulsing">(<i class="fas fa-expand-arrows-alt u-fa-padding" /> {{ pulseSpeedText }})</span>
  </span>
</template>

<style scoped>
.c-gamespeed {
  font-weight: bold;
  color: var(--color-text);
}
</style>
