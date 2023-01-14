<template>
  <div class="container">
    <h1 class="display-1">My Bank App</h1>
    <div class="row mb-4">
      <Box :amount="income" title="Income" />
      <Box :amount="expenses" title="Expenses" />
      <Box :amount="balance" title="Balance" />
    </div>

    <div class="d-flex mt-4">
      <input type="text" placeholder="add a new item" v-model="name" />
      <input type="number" placeholder="amount" v-model.number="amount" />
      <button @click="addItem" class="btn btn-info">Add</button>
    </div>

    <div id="search" class="d-flex mt-4">
      <input
        type="text"
        placeholder="search..."
        class="w-50"
        v-model="searchTerm"
      />
    </div>

    <!-- Trying to create checkboxes to filter the items -- all income or all expenses or both if nothing or both are selected -->
    <div class="form-check-inline">
      <label class="form-check-label">
        <input
          type="checkbox"
          class="form-check-input"
          v-model="incomeCheck"
        />Income only
      </label>
    </div>
    <div class="form-check-inline">
      <label class="form-check-label">
        <input
          type="checkbox"
          class="form-check-input"
          v-model="expenseCheck"
        />Expenses only
      </label>
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
      incomeCheck: false,
      expenseCheck: false,
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
    // For the boxes
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
    // For search filter & checkboxes
    filteredItems() {
      if (!incomeCheck && !expenseCheck) {
        return this.items;
      } else if (incomeCheck && !expenseCheck) {
        return this.items.filter((item) => item.amount > 0);
      } else if (!incomeCheck && expenseCheck) {
        return this.items.filter((item) => item.amount < 0);
      } else if (incomeCheck && expenseCheck) {
        return this.items;
      } else {
        return this.filteredItems.filter((item) =>
          item.name.toLowerCase().includes(this.searchTerm.toLowerCase())
        );
      }
      // return this.items.filter((item) =>
      //   item.name.toLowerCase(this.searchTerm).includes(this.searchTerm)
      // );
    },

    // For checkboxes -- This isn't quite working yet...revisit.
    /*Ok, so realistically, if I was making something with usability in mind, these checkboxes might not be the way to go.
    But, I'm curious how to make them work with the filter. Radio buttons might be a better option for this.

    OPTION 1:
    If the income-only button is selected, only income items will show in the filtered list.
    The expense-only button is now disabled.
    If the expense-only button is selected, only expenese items will show in the filtered list.
    The income-only button is now disabled.
    Both buttons cannot be selected at the same time.

    OPTION 2:
    If the income-only button is selected, only income items will show in the filtered list.
    If the expense-only button is selected, only expenese items will show in the filtered list.
    If both buttons are selected, all items will be listed.
    If neither button is selected, all items will be listed.*/

    // expenseItems() {
    //   if ((expenseCheck = true)) {
    //     return this.items.filter((item) => item.amount < 0);
    //   }
    //  },
  },
};
</script>

<style scoped>
/* Style done with Bootstrap: https://getbootstrap.com/docs/5.2/getting-started/introduction/ */
button {
  color: #ffff;
}
input {
  padding: 0.5rem;
  margin-right: 0.2rem;
}
</style>
