<template>
  <div class="grid">
    <bare-bones-header style="grid-area: header" />
    <order-status-customer class="status" style="grid-area: status" />
  </div>
</template>

<script>
import BareBonesHeader from "../components/BareBonesHeader.vue";
import OrderStatusCustomer from "../components/OrderStatusCustomer.vue";

export default {
  components: { BareBonesHeader, OrderStatusCustomer },
  setup() {},
};
</script>

<style scoped>
.grid {
  display: grid;
  grid-template-columns: 150px 1fr 100px;
  grid-template-rows: 220px 1fr 50px;
  grid-template-areas:
    "header header header"
    "status status status";
  height: 95vh;
  margin: 0;
  padding: 0;
  border: none;
  gap: 0;
}
.status {
  align-content: center;
  justify-content: center;
}
</style>
