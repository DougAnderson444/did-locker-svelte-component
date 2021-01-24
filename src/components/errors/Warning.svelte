<script>
  export let show;
</script>

{#if show}
  <div class="contain">
    <div class="card-container short">
      <div
        class="high"
        style="width: 500px; background-color: #fff3cd; border-color: #ffeeba;"
        padded
      >
        <slot name="phrase">
          <span>⛔️ Unknown error ⛔️</span>
        </slot>
      </div>
    </div>
  </div>
{/if}

<style>
  .contain {
    margin: 1em;
    width: 550px;
  }
</style>
