<script>
  import ProgressStatus from "../Utility/ProgressStatus.svelte";
  import * as pro from "../../js/process";
  import { DEFAULT_DID_DOC_TLD } from "../../js/constants.js";

  export let options;
  let TLD = options.tld || DEFAULT_DID_DOC_TLD;

  //svelte stores
  import { username, dnsLink, did } from "../../js/stores.js";

  const recordDNSLink = async () => {
    console.log(`Step 3: Create datkey={key}`);
    $dnsLink = await pro.recordDNSLink(
      $username,
      $did.replace("did:hypns:", ""),
      DEFAULT_DID_DOC_TLD
    );
  };

  $: if ($username && $did && !$dnsLink) recordDNSLink();
</script>

<ProgressStatus bind:awaiting={$dnsLink}>
  <span slot="before">Creating your DNS Link...</span>
  <span slot="complete">DNS Link created.</span>
</ProgressStatus>
