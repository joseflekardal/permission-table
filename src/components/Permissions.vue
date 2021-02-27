<template>
  <table>
    <thead>
      <tr>
        <th>id</th>
        <th>role</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="permission in permissions" :key="permission.id">
        <td>{{ permission.id }}</td>
        <td>{{ permission.role }}</td>
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
      this.permissions = [...this.modelValue];
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