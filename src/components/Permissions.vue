<template>
  <table>
    <thead>
      <tr>
        <th>ID</th>
        <th>Superadmin</th>
        <th>Kan se priser</th>
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
        <td>
          <input
            type="checkbox"
            :checked="!permission.permissionDenies.includes('SEE_PRICES')"
            @change="
              permission.permissionDenies.includes('SEE_PRICES')
                ? permission.permissionDenies.splice(
                    permission.permissionDenies.indexOf('SEE_PRICES'),
                    1
                  )
                : permission.permissionDenies.push('SEE_PRICES')
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