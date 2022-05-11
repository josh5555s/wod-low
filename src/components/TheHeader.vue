<template>
  <nav>
    <button
      @click="updateButtons('missing')"
      :class="{ missing: isActive('missing') }"
    >
      Missing Expiration Dates
    </button>
    <button
      @click="updateButtons('upcoming')"
      :class="{ upcoming: isActive('upcoming') }"
    >
      Upcoming Expirations
    </button>
    <button @click="updateButtons('low')" :class="{ low: isActive('low') }">
      Low Counts
    </button>
  </nav>
</template>

<script>
export default {
  data() {
    return {
      activeButton: "missing",
    };
  },
  computed: {},
  methods: {
    updateButtons(clickedButton) {
      this.activeButton = clickedButton;
      this.$emit("active-button", clickedButton);
    },
    isActive(buttonName) {
      return buttonName === this.activeButton;
    },
  },
  created() {
    this.$emit("active-button", "missing");
  },
  mounted() {},
};
</script>

<style>
.missing {
  background-color: lightgray;
}
.upcoming {
  background-color: lightgray;
}
.low {
  background-color: lightgray;
}
</style>
