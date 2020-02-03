<script>
  import { setContext } from "svelte";
  // components
  import Navbar from "./Navbar.svelte";
  import ExpensesList from "./ExpensesList.svelte";
  import Totals from "./Totals.svelte";
  import ExpenseForm from "./ExpenseForm.svelte";
  // data
  import expensesData from "./expenses.js";
  // variables
  let expenses = [...expensesData];
  // set editing variables
  let setName = '';
  let setAmount = null;
  let setID = null;
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
  function addExpense({ name, amount }) {
    let expense = { id: Math.random() * Date.now(), name, amount };
    expenses = [expense, ...expenses];
  }
  function setModifiedExpense(id) {
    let expense = expenses.find(item => item.id === id);
    console.log(expense);
    
    setID = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
    console.log({setID, setName, setAmount});
    
  }
  // context
  setContext("remove", removeExpense);
  setContext("clearExpenses", clearExpenses);
  setContext("modify", setModifiedExpense);
</script>

<Navbar />
<main class="content">
  <ExpenseForm {addExpense} />
  <Totals title="total expenses" {total} } />
  <ExpensesList {expenses} />
</main>
