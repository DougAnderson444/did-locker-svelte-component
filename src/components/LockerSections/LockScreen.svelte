<script>
  //svelte stores
  import { wallet, lockerSection, dnsLink } from "../../js/stores.js";
  import Warning from "../errors/Warning.svelte";
  import Spinner from "../Utility/Spinner.svelte";

  const LOCK_TYPE = "passphrase";

  let loading = false;
  let error = undefined;
  let value = "";

  const unlock = (lockType, challenge) => {
    loading = true;

    $wallet.locker
      .getLock(lockType)
      .unlock(challenge)
      .then(() => {
        $lockerSection = "LockerContents";
      })
      .catch((err) => {
        loading = false;
        console.log(`unlock err: ${err}`);
        error = err;
      });
  };

  const handleInputKeyPress = (event) => {
    if (event.charCode === 13) {
      if (!$wallet.locker.isPristine()) unlock(LOCK_TYPE, value);
      else if ($dnsLink)
        console.log(`compare with DID Doc then get from ipld network`); // TODO
    }
  };
</script>

<div class="contain">
  <div>
    <form class="form" on:submit|preventDefault={handleInputKeyPress}>
      <h1>Unlock your data locker</h1>
      <input
        bind:value
        type="password"
        id="password"
        name="password"
        variant="outlined"
        label="Passphrase"
        aria-controls="super-helper"
        aria-describedby="super-helper"
        autocomplete="current-password"
        on:keypress={handleInputKeyPress}
      />
      <label for="password" id="super-helper"
        >Enter your passphrase to unlock</label
      >

      <Warning show={error}>
        <span slot="phrase">⛔️ {error.message} ⛔️</span>
      </Warning>
    </form>
  </div>
  {#if loading}
    <Spinner />
  {/if}
</div>

<style>
  .contain {
    margin: 1em 0 1em 0;
    width: 550px;
  }
</style>
