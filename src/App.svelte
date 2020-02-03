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
  let setName = "";
  let setAmount = null;
  let setID = null;
  // reactive
  $: isEditing = setID ? true : false;
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
    setID = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
  }
  function editExpense({ name, amount }) {
    console.log({ name, amount });
  }
  // context
  setContext("remove", removeExpense);
  setContext("clearExpenses", clearExpenses);
  setContext("modify", setModifiedExpense);
</script>

<Navbar />
<main class="content">
  <ExpenseForm
    name={setName} amount={setAmount}
    {addExpense} {isEditing} {editExpense} />
  <Totals title="total expenses" {total} />
  <ExpensesList {expenses} />
</main>
