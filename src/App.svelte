<script>
  import { setContext, onMount, afterUpdate } from "svelte";
  // components
  import Navbar from "./Navbar.svelte";
  import ExpensesList from "./ExpensesList.svelte";
  import Totals from "./Totals.svelte";
  import ExpenseForm from "./ExpenseForm.svelte";
  // data
  // import expensesData from "./expenses.js";
  // variables
  let expenses = [];
  // set editing variables
  let setName = "";
  let setAmount = null;
  let setId = null;
  // toggle form variables
  let isFormOpen = false;
  // reactive
  $: isEditing = setId ? true : false;
  $: total = expenses.reduce((accumulator, current) => {
    return (accumulator += current.amount);
  }, 0);
  // functions
  function showForm() {
    isFormOpen = true;
  }
  function hideForm() {
    isFormOpen = false;
    setId = null;
    setName = "";
    setAmount = null;
  }
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
    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
    showForm();
  }
  function editExpense({ name, amount }) {
    expenses = expenses.map(item => {
      return item.id === setId ? { ...item, name, amount } : { ...item };
    });
    setId = null;
    setName = "";
    setAmount = null;
  }
  // context
  setContext("remove", removeExpense);
  setContext("clearExpenses", clearExpenses);
  setContext("modify", setModifiedExpense);
  // local storage
  function setLocalStorage() {
    localStorage.setItem("expenses", JSON.stringify(expenses));
  }
  onMount(() => {
    expenses = localStorage.getItem("expenses")
      ? JSON.parse(localStorage.getItem("expenses"))
      : [];
  });
  afterUpdate(()=> {
    console.log("after update");
    setLocalStorage();
  })
</script>

<Navbar {showForm} />
<main class="content">
  {#if isFormOpen}
    <ExpenseForm
      name={setName}
      amount={setAmount}
      {addExpense}
      {isEditing}
      {editExpense}
      {hideForm} />
  {/if}
  <Totals title="total expenses" {total} />
  <ExpensesList {expenses} />
</main>
