<template>
  <div id="app">
    <Customers v-model="permissions" />
    <Permissions v-model="permissions" />
    <pre>{{ permissions }}</pre>
  </div>
</template>

<script>
import Permissions from "./Permissions";
import Customers from "./Customers";
export default {
  name: "app",
  components: {
    Permissions,
    Customers,
  },
  data() {
    return {
      permissions: [
        { id: "222222", permissionDenies: [], role: "BUYER_ADMIN" },
      ],
    };
  },
};
</script>
