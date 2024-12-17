<script>
export default {
  name: "ClassicSubtabButton",
  props: {
    subtab: {
      type: Object,
      required: true
    },
    parentName: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      isAvailable: false,
      hasNotification: false,
      isCurrentSubtab: false,
      tabName: ""
    };
  },
  computed: {
    classObject() {
      return {
        "o-tab-btn": true,
        "o-tab-btn--secondary o-tab-btn--secondary--bordered": true,
        "o-tab-btn--secondary-small": this.subtab.name.length > 17,
        "o-subtab-btn--active": this.isCurrentSubtab,
        "o-tab-btn--infinity": this.parentName === "Infinity",
        "o-tab-btn--eternity": this.parentName === "Eternity",
        "o-tab-btn--reality": this.parentName === "Reality",
        "o-tab-btn--armageddon": this.parentName === "Armageddon",
        "o-tab-btn--celestial": this.parentName === "Celestials"
      };
    },
  },
  methods: {
    update() {
      this.isAvailable = this.subtab.isAvailable;
      this.hasNotification = this.subtab.hasNotification;
      this.isCurrentSubtab = this.subtab.isOpen && Theme.currentName() !== "S9";

      this.tabName = Pelle.transitionText(
        this.subtab.name,
        this.subtab.name,
        Math.max(Math.min(GameEnd.endState - (this.subtab.id) % 4 / 10, 1), 0))
    },
    tabname(){
      return this.subtab.symbol +" "+Pelle.transitionText(
        this.subtab.name,
        this.subtab.name,
        Math.max(Math.min(GameEnd.endState - (this.subtab.id) % 4 / 10, 1), 0)
      )+" "+this.subtab.symbol;
    },
    hover() {
      AudioManagement.playSound("click_hover",undefined,1.1)
    }
  },
};
</script>

<template>
  <button
    v-if="isAvailable"
    :class="classObject"
    @click="subtab.show(true)"
    @mouseenter="hover()"
  >
  <span v-html="tabName"></span>
  <div v-if="hasNotification" class="fas fa-circle-exclamation l-notification-icon"/>
  </button>
</template>

<style scoped>
.o-tab-btn {
  position: relative;
  height: 2.5rem;
  vertical-align: middle;
  padding-top: 0.2rem;
}

.o-subtab-btn--active {
  height: 2.5rem;
}

.s-base--metro .o-subtab-btn--active {
  border-bottom-width: 0.4rem;
}
</style>
