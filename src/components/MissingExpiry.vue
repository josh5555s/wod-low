<template>
  <div>
    <ul v-for="store in stores" :key="store">
      <li @click="toggleExpandedStore(store)">
        <a>
          {{ store }}
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
  <!-- <div>
    <ul v-for="store in stores" :key="store">
      <li>
        {{ store }}
        <ul v-for="type in productTypes" :key="type">
          <li>
            <span>
              {{ type }}
            </span>
            <ul v-for="item in items(store, type)" :key="item">
              <span>
                <li>
                  {{ item.productName }}
                </li>
              </span>
            </ul>
          </li>
        </ul>
      </li>
    </ul>
  </div> -->
</template>

<script>
export default {
  props: ["inventory"],
  data() {
    return {
      expansions: {},
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
        console.log(item.expiration);
        return item.expiration === "";
      });
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
      return this.expansions[store].expanded.store ? true : false;
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
        console.log("toggleShowFullItem", store, productType, i);
      } else {
        this.expansions[store][productType][i].full =
          !this.expansions[store][productType][i].full;
      }
    },
  },
  created() {
    this.initExpansionsObj();
    this.initExpandedProps();
  },
  mounted() {},
  watch: {
    inventory() {
      this.initExpansionsObj();
      this.initExpandedProps();
    },
  },
};
</script>

<style>
</style>
