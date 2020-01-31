<script>
  import { getContext } from "svelte";
  // components
  import SectionTitle from "./Title.svelte";
  import Expense from "./Expense.svelte";
  export let expenses = [];
  // context
  const clearExpenses = getContext("clearExpenses");
</script>

<style>
  h2 {
    text-transform: capitalize;
  }
</style>

<section>
  <SectionTitle title="expense list" />
  <ul>
    {#each expenses as expense, index}
      <Expense {...expense} />
    {:else}
      <h2>no expenses to list</h2>
    {/each}
    <button
      type="button"
      class="btn btn-primary btn-block"
      on:click={clearExpenses}>
      clear expenses
    </button>
  </ul>
</section>
