<template>
  <div class="container">
    <h1>My Bank App</h1>
    <div class="row mb-4">
      <Box :amount="income" title="Income" />
      <Box :amount="expenses" title="Expenses" />
      <Box :amount="balance" title="Balance" />
    </div>

    <div class="d-flex mt-4">
      <input type="text" placeholder="add a new item" v-model="name" />
      <input type="number" placeholder="amount" v-model.number="amount" />
      <button @click="addItem">Add</button>
    </div>

    <div class="d-flex mt-4">
      <input type="text" placeholder="search..." v-model="searchTerm" />
    </div>
    <div>
      <ul class="mt-4 list-group">
        <li
          v-for="item in filteredItems"
          :key="item.id"
          class="list-group-item d-flex justify-content-between align-item-center"
        >
          <span>{{ item.name }}</span>
          <span
            :class="{
              'text-success': item.amount > 0,
              'text-danger': item.amount < 0,
            }"
            >{{ item.amount }}</span
          >
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import Box from "./components/Box.vue";
export default {
  name: "App",
  components: {
    Box,
  },
  data() {
    return {
      name: "",
      amount: 0,
      id: 4,
      searchTerm: "",
      items: [
        { id: 1, name: "Food", amount: -30 },
        { id: 2, name: "Groceries", amount: -42 },
        { id: 3, name: "Salary", amount: 130 },
      ],
    };
  },
  methods: {
    addItem() {
      const newItem = {
        id: this.id,
        name: this.name,
        amount: this.amount,
      };
      this.items.push(newItem);
      this.id++;
      this.name = "";
      this.amount = 0;
    },
  },
  computed: {
    income() {
      return this.items
        .filter((i) => i.amount > 0)
        .reduce((acc, b) => acc + b.amount, 0);
    },
    expenses() {
      return this.items
        .filter((i) => i.amount < 0)
        .reduce((acc, b) => acc + b.amount, 0);
    },
    balance() {
      return this.income + this.expenses;
    },
    filteredItems() {
      return this.items.filter((item) => item.name.includes(this.searchTerm));
    },
  },
};
</script>

<style scoped>
/* Style done with Bootstrap: https://getbootstrap.com/docs/5.2/getting-started/introduction/ */
</style>
