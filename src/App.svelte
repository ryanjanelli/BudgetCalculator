<script>
  import { setContext } from "svelte";
  // components
  import Navbar from "./Navbar.svelte";
  import ExpensesList from "./ExpensesList.svelte";
  // data
  import expensesData from "./expenses.js";
  // variables
  let expenses = [...expensesData];
  // functions
  function removeExpense(id) {
    expenses = expenses.filter(item => item.id !== id);
  }
  function clearExpenses() {
    expenses = [];
  }
  // context
  setContext("remove", removeExpense);
  setContext("clearExpenses", clearExpenses);
</script>

<Navbar />
<main class="content">
  <ExpensesList {expenses} />
</main>
