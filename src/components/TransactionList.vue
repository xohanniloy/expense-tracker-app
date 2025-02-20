<script setup>
import { ref, onMounted } from "vue";
import "bootstrap/dist/css/bootstrap.min.css";

const title = ref("");
const amount = ref(0);
const type = ref("income");

const transactions = ref([]);
onMounted(() => {
  const savedTransactions = localStorage.getItem("transactions");
  if (savedTransactions) {
    transactions.value = JSON.parse(savedTransactions);
  }
});
const addTransaction = () => {
  if (!title.value || !amount.value || amount.value <= 0) {
    alert("Please enter valid details!");
    return;
  }
  const newTransaction = {
    id: Date.now(),
    title: title.value,
    amount: amount.value,
    type: type.value,
  };
  transactions.value.push(newTransaction);
  localStorage.setItem("transactions", JSON.stringify(transactions.value));

  title.value = "";
  amount.value = "";
  type.value = "income";
};
const deleteTransaction = (id) => {
  transactions.value = transactions.value.filter((transaction) => {
    return transaction.id !== id;
  });
  localStorage.setItem("transactions", JSON.stringify(transactions.value));
};


const selectedFilter = ref("all");


</script>

<template>
  <div class="container g-0 bg-dark mt-5 p-4 text-white">
    <div class="row mb-2">
      <div class="col-3">
        <input
          type="text"
          class="form-control"
          placeholder="Title"
          v-model="title"
        />
      </div>
      <div class="col-3">
        <input
          type="number"
          class="form-control"
          placeholder="Amount"
          v-model="amount"
        />
      </div>
      <div class="col-3">
        <select class="form-select" v-model="type">
          <option value="income">Income</option>
          <option value="expense">Expense</option>
        </select>
      </div>
      <div class="col-3">
        <button class="btn btn-primary w-100" @click="addTransaction">
          Add
        </button>
      </div>
    </div>

    <div class="row">
      <div
        class="d-flex justify-content-between align-items-center my-3 text-white"
      >
        <div class="form-group mb-0">
          <div class="form-check form-check-inline">
            <input
              type="radio"
              name="type"
              class="form-check-input"
              id="filter-all"
              value="all"
              v-model="selectedFilter"
            /><label for="filter-all" class="form-check-label">All</label>
          </div>
          <div class="form-check form-check-inline">
            <input
              type="radio"
              name="type"
              class="form-check-input"
              id="filter-income"
              value="income"
              v-model="selectedFilter"
            /><label for="filter-income" class="form-check-label">Income</label>
          </div>
          <div class="form-check form-check-inline">
            <input
              type="radio"
              name="type"
              class="form-check-input"
              id="filter-expense"
              value="expense"
              v-model="selectedFilter"
            /><label for="filter-expense" class="form-check-label"
              >Expense</label
            >
          </div>
        </div>
      </div>
    </div>

    <div class="container g-0 mt-3">
      <table class="table table-dark table-striped table-bordered">
        <thead>
          <tr>
            <th scope="col">Title</th>
            <th scope="col">Amount</th>
            <th scope="col">Type</th>
            <th scope="col">Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="transaction in transactions" :key="transaction.id">
            <td>{{ transaction.title }}</td>
            <td
              :class="
                transaction.type == 'income' ? 'text-success' : 'text-danger'
              "
            >
              ${{ transaction.amount }}
            </td>
            <td class="text-uppercase">{{ transaction.type }}</td>
            <td>
              <button
                class="btn btn-danger w-100"
                @click="deleteTransaction(transaction.id)"
              >
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped></style>
