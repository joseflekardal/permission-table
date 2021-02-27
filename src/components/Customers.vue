<template>
  <section>
    <label
      v-for="customer in customers"
      :key="customer.id"
      style="display: block"
    >
      <input
        type="checkbox"
        :checked="inSelected(customer.id)"
        @change="toggleCustomer(customer)"
      />
      {{ customer.id }}
    </label>
  </section>
</template>

<script>
const makePermission = (customer) => {
  return {
    role: "USER",
    permissionDenies: [],
    ...customer,
  };
};
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
      customers: [
        { id: "111111" },
        { id: "111111200" },
        { id: "111111300" },
        { id: "222222" },
        { id: "333333" },
      ],
      selected: [...this.modelValue],
    };
  },
  methods: {
    inSelected(customerId) {
      return !!this.selected.find((x) => x.id === customerId);
    },
    toggleCustomer(customer) {
      if (!this.inSelected(customer.id)) {
        let subCustomers = [];
        if (customer.id.length < 9) {
          subCustomers = this.subCustomers
            .filter((c) => {
              return c.id.startsWith(customer.id) && !this.inSelected(c.id);
            })
            .map(makePermission);
        }

        this.selected.push(makePermission(customer), ...subCustomers);
        return;
      }

      if (customer.id.length === 6) {
        this.selected = this.selected.filter((c) => {
          return !c.id.startsWith(customer.id);
        });
      } else {
        this.selected.splice(
          this.selected.findIndex((c) => customer.id === c.id),
          1
        );
      }
    },
  },
  computed: {
    subCustomers() {
      return this.customers.filter((customer) => customer.id.length === 9);
    },
  },
  watch: {
    selected() {
      this.$emit("input", [...this.selected]);
    },
  },
};
</script>