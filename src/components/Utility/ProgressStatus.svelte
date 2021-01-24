<script>
  export let awaiting;
</script>

<span class="progress">
  {#if !awaiting}
    <span class="pending" style="margin-right: 4px; color: #ddd;">
      pending
    </span>
  {:else}
    <span class="done" style="margin-right: 4px; color: green;">
      check_circle
    </span>
  {/if}
</span>
<slot name="before">
  <span />
</slot>

{#if awaiting}
  {#await awaiting then awaiting}
    <slot name="complete">
      <div />
    </slot>
  {/await}
{/if}
<br />

<style>
  .progress {
    position: relative;
    top: 0.35em;
    left: 0;
  }
  .pending:before {
    content: "\f2f1";
    font: normal normal normal 1.5em/1 FontAwesome;
    color: #ccc;
    padding-right: 0.1em;
    margin-right: 0.1em;
  }
  .done:before {
    content: "\f00c";
    font: normal normal normal 1.5em/1 FontAwesome;
    color: #ccc;
    padding-right: 0.1em;
    margin-right: 0.1em;
  }
</style>
