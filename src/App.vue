<template>
  <TheHeader />
  <LowCounts />
</template>

<script>
import TheHeader from "./components/TheHeader.vue";
import LowCounts from "./components/LowCounts.vue";
export default {
  components: {
    TheHeader,
    LowCounts,
  },
  data() {
    return {};
  },
  computed: {},
  methods: {},
  created() {},
  mounted() {},
};
</script>
<style>
* {
  font-family: "Montserrat", sans-serif;
  font-weight: thin;
  text-transform: capitalize;
}

ul {
  padding-left: 20px;
}

p {
  display: flex;
  flex-direction: column;
}

a {
  text-decoration: none;
}

.product-list {
  margin: 10px 0 10px 0;
}

.item-list {
  margin: 10px 0 10px 0;
}
</style>
