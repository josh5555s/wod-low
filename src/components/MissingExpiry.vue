<template>
  <div>
    <ul v-for="store in stores" :key="store">
      <li>
        {{ store }}
        <ul v-for="type in productTypes" :key="type">
          <li class="hidden">
            <span class="hidden">
              {{ type }}
            </span>
            <ul v-for="item in items(store, type)" :key="item">
              <span class="hidden">
                <li>{{ item.productName }}</li>
              </span>
            </ul>
          </li>
        </ul>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ["inventory", "stores", "productTypes"],
  data() {
    return {};
  },
  computed: {},
  methods: {
    items(store, productType) {
      return this.inventory[store][productType];
    },
  },
  created() {},
  mounted() {},
};
</script>

<style>
.hidden {
  display: none;
}
</style>
