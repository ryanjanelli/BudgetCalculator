<script>
  import { setContext } from "svelte";
  // components
  import Navbar from "./Navbar.svelte";
  import ExpensesList from "./ExpensesList.svelte";
  import Totals from "./Totals.svelte";
  // data
  import expensesData from "./expenses.js";
  // variables
  let expenses = [...expensesData];
  // reactive
  $: total = expenses.reduce((accumulator, current) => {
    return (accumulator += current.amount);
  }, 0);
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
  <Totals title="total expenses" {total} } />
  <ExpensesList {expenses} />
</main>
