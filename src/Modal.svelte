<script>
  import { blur, fade, fly } from "svelte/transition";
</script>

<div class="modal-container" in:blur out:fade>
  <div class="modal-content" in:fly={{y:200, delay:200}} out:fly={{y:-100}}>
    <slot>
      <!-- optional -->
    </slot>
  </div>
</div>
