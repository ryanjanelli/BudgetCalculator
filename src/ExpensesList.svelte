<script>
  import { getContext } from "svelte";
  import { fly } from "svelte/transition";
  import { flip } from "svelte/animate";
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
    {#each expenses as expense, index (expense.id)}
      <div in:fly={{ x: 200, delay: (index + 1) * 400 }} out:fly={{ x: -200}} animate:flip >
        <Expense {...expense} />
      </div>
    {:else}
      <h2>no expenses to list</h2>
    {/each}
    <button
      type="button"
      class="btn btn-primary btn-block"
      on:click={clearExpenses} >
      clear expenses
    </button>
  </ul>
</section>
