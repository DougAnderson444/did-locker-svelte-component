<script>
  import ProgressStatus from "../Utility/ProgressStatus.svelte";
  import * as pro from "../../js/process";

  //svelte stores
  import { username, did } from "../../js/stores.js";

  let pageSaved = false;

  const savePage = async () => {
    console.log(`Step 5: Save username & DID to Fauna`);
    pageSaved = await pro.savePage({
      username: $username,
      did: $did,
    });
  };

  $: if ($username && $did) savePage();
</script>

<ProgressStatus bind:awaiting={pageSaved}>
  <span slot="before">Saving your page...</span>
  <span slot="complete">Page saved.</span>
</ProgressStatus>
