<template>
  <div>
    <ul v-for="store in stores" :key="store">
      <li @click="toggleExpandedStore(store)">
        <a>
          {{ store }}
          {{ storeItemsCount(store) }}
        </a>
      </li>
      <ul
        v-for="productType in productTypes(store)"
        :key="productType"
        class="product-list"
      >
        <li
          v-if="isStoreExpanded(store)"
          @click="toggleExpandedProductType(store, productType)"
        >
          <a>
            {{ productType }}
            ({{ items(store, productType).length }})
          </a>
        </li>
        <ul
          v-for="(item, i) in items(store, productType)"
          :key="item"
          class="item-list"
        >
          <div
            v-if="
              isStoreExpanded(store) && isProductExpanded(store, productType)
            "
            @click="toggleShowFullItem(store, productType, i)"
          >
            <a>
              <li>Name: {{ item.productName }}</li>
              <div v-if="expansions[store][productType][i].full === true">
                <li>Brand: {{ item.farm }}</li>
                <li>Price: ${{ item.price }}</li>
                <li>Feel: {{ item.type }}</li>
              </div>
            </a>
          </div>
        </ul>
      </ul>
    </ul>
  </div>
</template>

<script>
export default {
  props: ["inventory"],
  data() {
    return {
      expansions: {},
      initialized: false,
    };
  },
  computed: {
    stores() {
      return Object.keys(this.inventory);
    },
  },
  methods: {
    productTypes(store) {
      return Object.keys(this.inventory[store]);
    },
    items(store, productType) {
      return this.inventory[store][productType].filter((item) => {
        return item.expiration === "";
      });
    },
    storeItemsCount(store) {
      let count = 0;
      this.productTypes(store).forEach((type) => {
        count += this.items(store, type).length;
      });
      return `(${count})`;
    },
    initialize() {
      if (!this.initialized) {
        this.initExpansionsObj();
        this.initExpandedProps();
        this.initialized = true;
      }
    },
    initExpansionsObj() {
      this.expansions = JSON.parse(JSON.stringify(this.inventory));
    },
    initExpandedProps() {
      this.stores.forEach((store) => {
        this.expansions[store].expanded = {};
        this.expansions[store].expanded.store = false;
        this.productTypes(store).forEach((productType) => {
          this.expansions[store].expanded[productType] = false;
        });
      });
    },
    isStoreExpanded(store) {
      if (this.initialized) {
        return this.expansions[store].expanded.store ? true : false;
      } else {
        return false;
      }
    },
    isProductExpanded(store, productType) {
      return this.expansions[store].expanded[productType] ? true : false;
    },
    toggleExpandedStore(store) {
      return (this.expansions[store].expanded.store =
        !this.expansions[store].expanded.store);
    },
    toggleExpandedProductType(store, productType) {
      return (this.expansions[store].expanded[productType] =
        !this.expansions[store].expanded[productType]);
    },
    toggleShowFullItem(store, productType, i) {
      if ("full" in this.expansions[store][productType][i] == false) {
        this.expansions[store][productType][i].full = true;
      } else {
        this.expansions[store][productType][i].full =
          !this.expansions[store][productType][i].full;
      }
    },
  },
  created() {
    setTimeout(() => {
      this.initialize();
    }, 200);
  },
  mounted() {},
  // watch: {
  //   inventory() {
  //     this.initialize();
  //   },
  // },
};
</script>

<style>
</style>
