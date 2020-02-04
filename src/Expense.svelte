<script>
  import { blur, slide, scale, fade, fly } from "svelte/transition";
  import { quintOut } from "svelte/easing";
  import { getContext } from "svelte";
  // components
  export let id;
  export let name = "";
  export let amount = 0;
  let displayAmount = false;
  // functions
  function toggleAmount() {
    displayAmount = !displayAmount;
  }
  // context
  const removeExpense = getContext("remove");
  const setModifiedExpense = getContext("modify");
  const showForm = getContext("showForm");
</script>

<article class="single-expense">

  <div class="expense-info">
    <h2>
      {name}
      <button class="amount-btn" on:click={toggleAmount}>
        <i class="fas fa-caret-down" />
      </button>
    </h2>
    {#if displayAmount}
      <h4 in:fly={{ x: 200, y: 0, duration: 1000, delay: 200, easing:quintOut }} out:slide={{easing:quintOut}}>
        amount : $ {amount}
      </h4>
    {/if}
  </div>
  <div class="expense-buttons">
    <button
      class="expense-btn edit-btn"
      on:click={() => setModifiedExpense(id)}>
      <i class="fas fa-pen" />
    </button>
    <button class="expense-btn delete-btn" on:click={() => removeExpense(id)}>
      <i class="fas fa-trash" />
    </button>
  </div>
</article>
