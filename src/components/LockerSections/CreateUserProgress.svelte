<script>
  import { lockerSections } from "./index.js";

  const backgroundComponents = [
    lockerSections["CreateIdentity"],
    lockerSections["CreateDNSLink"],
    lockerSections["CreateDatabaseEntry"],
    lockerSections["CreateDataServiceEntry"],
  ];
</script>

Creating details:
<br />
{#if backgroundComponents && backgroundComponents.length > 0}
  {#each backgroundComponents as background}
    <svelte:component this={background.component} />
  {/each}
{/if}

<style>
</style>
