<template>
  <table>
    <thead>
      <tr>
        <th>ID</th>
        <th>Superadmin</th>
        <th v-for="permission in allPermissions" :key="permission.value">
          {{ permission.label }}
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="permission in permissions" :key="permission.id">
        <td>{{ permission.id }}</td>
        <td>
          <input
            type="checkbox"
            :checked="permission.role === 'BUYER_ADMIN'"
            @change="
              permission.role =
                permission.role === 'BUYER_ADMIN' ? 'USER' : 'BUYER_ADMIN'
            "
          />
        </td>
        <td v-for="p in allPermissions" :key="p.value">
          <input
            type="checkbox"
            :checked="!permission.permissionDenies.includes(p.value)"
            @change="
              permission.permissionDenies.includes(p.value)
                ? permission.permissionDenies.splice(
                    permission.permissionDenies.indexOf(p.value),
                    1
                  )
                : permission.permissionDenies.push(p.value)
            "
          />
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  model: {
    prop: "modelValue",
  },
  props: {
    modelValue: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      permissions: [...this.modelValue],
      allPermissions: [
        { label: "Kan se priser", value: "SEE_PRICES" },
        { label: "Kan ändra sortiment", value: "CHANGE_PRODUCTS" },
        { label: "Kan se rabatter", value: "SEE_DISCOUNT" },
        { label: "Kan lägga order", value: "PLACE_ORDER" },
        { label: "Kan se ekonomisk uppföljning", value: "SEE_ECONOMY" },
        { label: "Kan se orderhistorik", value: "SEE_ORDER_HISTORY" },
        { label: "Kan skapa offerter", value: "CREATE_OFFERT" },
      ],
    };
  },
  watch: {
    modelValue() {
      if (this.permissions.length !== this.modelValue.length) {
        this.permissions = [...this.modelValue];
      }
    },
    permissions() {
      this.$emit("input", [...this.permissions]);
    },
  },
};
</script>

<style>
table {
  border-collapse: collapse;
}

td,
th {
  border: 1px solid grey;
  padding: 0.5em;
  text-align: left;
}

th {
  background: grey;
  color: #fff;
}
</style>