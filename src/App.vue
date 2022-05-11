<template>
  <TheHeader @active-button="updateActiveButton" />

  <MissingExpiry v-if="activeButton === 'missing'" :inventory="inventory" />
  <UpcomingExpiry
    v-else-if="activeButton === 'upcoming'"
    :inventory="inventory"
  />
  <LowCounts v-else-if="activeButton === 'low'" :inventory="inventory" />
</template>

<script>
import TheHeader from "./components/TheHeader.vue";
import MissingExpiry from "./components/MissingExpiry.vue";
import UpcomingExpiry from "./components/UpcomingExpiry.vue";
import LowCounts from "./components/LowCounts.vue";
export default {
  components: {
    TheHeader,
    MissingExpiry,
    UpcomingExpiry,
    LowCounts,
  },
  data() {
    return {
      inventory: {},
      activeButton: "",
    };
  },
  computed: {},
  methods: {
    updateActiveButton(clickedButton) {
      this.activeButton = clickedButton;
    },
    async getInventory() {
      try {
        fetch("https://api.westernoregondispensary.com/allInventory", {
          method: "GET",
          accept: "application/json",
          headers: {
            "Content-Type": "application/json;charset=UTF-8",
          },
        })
          .then((response) => {
            if (response.ok) {
              console.log("response is OK");
              return response.json();
            } else {
              console.log(response);
            }
          })
          .then((inventory) => {
            this.inventory = JSON.parse(inventory);
          })
          .catch((error) => {
            console.log(error);
          });
      } catch (error) {
        this.error = error.message || "Failed to get inventory";
      }
    },
  },
  created() {
    this.getInventory();
  },
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
