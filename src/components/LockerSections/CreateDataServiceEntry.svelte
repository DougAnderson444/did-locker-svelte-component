<!--<script>
  import * as pro from "../../js/process";
  import { DEFAULT_DID_DOC_TLD, ROOT_CID_PEM } from "../../js/constants.js";
  import ObjectComp from "../Utility/ObjectComp.svelte";
  import ProgressStatus from "../Utility/ProgressStatus.svelte";

  //svelte stores
  import {
    username,
    password,
    wallet,
    rootCidPem,
    dnsLink,
    identity,
    did,
    drive,
  } from "../../js/stores.js";
  const LOCK_TYPE = "passphrase";
  let pageSaved = false;
  let didDoc;
  let serviceEndpoint;

  const addService = async () => {
    console.log(`Step 6: Create data service`);

    let backup = await $identity.backup.getData();

    serviceEndpoint = `hypns://${drive.key.toString("hex")}/data/`;
    didDoc = await $wallet.identities.addService(
      "hyper",
      { drive },
      {
        id: `data`,
        type: "DataHub",
        serviceEndpoint,
      }
    );
  };

  $: if ($identity && $wallet.identities.isLoaded() && $did) addService();
</script><ProgressStatus bind:awaiting={didDoc}>
  <span slot="before">Creating your data id...</span>
  <div slot="complete">
    Data ID created. {serviceEndpoint}
    <br />
    DIDDoc updated
    <br />
    <ObjectComp val={didDoc} key="DID Document (Click to expand)" />
  </div>
</ProgressStatus> -->
