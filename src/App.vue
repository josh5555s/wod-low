<template>
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
      <ul v-for="(item, i) in items(store, productType)" :key="item" class="item-list">
        <div
          v-if="isStoreExpanded(store) && isProductExpanded(store, productType)"
          @click="toggleShowFullItem(store, productType, i)"
        >
          <a>
            <li>Name: {{ item.productName }}</li>
            <li>Remaining: {{ item.remaining }}</li>
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
</template>

<script>
export default {
  data() {
    return {
      inventory: {},
      stores: [],
      expansions: {},
    };
  },
  computed: {},
  methods: {
    getLowInventory() {
      console.log('getting low inventory!');
      fetch('https://api.westernoregondispensary.com/lowInventory')
        // fetch("http://192.168.1.2:4000/lowInventory")
        .then((response) => response.json())
        .then((data) => {
          this.inventory = JSON.parse(data);
          this.stores = Object.keys(this.inventory);
          this.initExpansionsObj();
          this.initExpandedProps();
          console.log(this.inventory);
        });
    },
    productTypes(store) {
      return Object.keys(this.inventory[store]);
    },
    items(store, productType) {
      return Object.values(this.inventory[store][productType]);
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
      if ('full' in this.expansions[store][productType][i] == false) {
        this.expansions[store][productType][i].full = true;
        console.log('toggleShowFullItem', store, productType, i);
      } else {
        this.expansions[store][productType][i].full =
          !this.expansions[store][productType][i].full;
      }
    },
  },
  created() {
    this.getLowInventory();
  },
  mounted() {},
};
</script>
<style>
* {
  font-family: 'Montserrat', sans-serif;
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
