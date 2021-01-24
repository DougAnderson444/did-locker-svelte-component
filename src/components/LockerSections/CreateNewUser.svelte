<script>
  import { PasswordError } from "../errors";
  import InvalidWarning from "../errors/InvalidWarning.svelte";
  import Warning from "../errors/Warning.svelte";
  import checkPassphraseStrength from "did-locker/src/locker/locks/utils/passphrase-strength";

  // svelte stuff
  import { onMount } from "svelte";
  import {
    wallet,
    lockerSection,
    username,
    password,
    error,
  } from "../../js/stores";

  //local state vars
  let loading;
  let validation = {};
  let match = true;
  let checked = false;
  let agreements = [];
  let confirmValue = "";

  if (!$username) {
    let isDev = window.location.hostname.includes("localhost");
    let splitHost = window.location.hostname.split(".");

    if (
      (!isDev && splitHost.length === 3) ||
      (isDev && splitHost.length === 2)
    ) {
      $username = splitHost[0];
    }
  }

  const LOCK_TYPE = "passphrase";

  async function validate(passphrase) {
    const result = checkPassphraseStrength(passphrase);
    if (result.score < 0.5) {
      throw new PasswordError("Passphrase is too weak", result);
    }
    return result;
  }

  const validateStrength = (lockType, solution) => {
    validate(solution)
      .then((v) => {
        validation = v;
      })
      .catch((error) => {
        validation = {
          score: error.score,
          suggestions: error.suggestions,
          warning: error.warning,
          error: true,
        };
      });
  };

  const handlePasswordInput = (event) => {
    validateStrength(LOCK_TYPE, $password);
  };

  const handleInputKeyPress = (event) => {
    if (event.charCode === 13 && !validation.error && $password) {
      handleSubmit();
    }
  };

  const checkMatch = async () => {
    confirmValue === $password ? (match = true) : (match = false);
  };

  const handleSubmit = () => {
    $error = false; //reset
    if (!validation.error && $password && match) {
      // process creation
      // Take PeerId privKey and encrypt with this password
      console.log(`Step 1. Enable  wallet`);
      $wallet.locker
        .getLock(LOCK_TYPE)
        .enable($password)
        .then(() => {
          $lockerSection = "SetupDevice";
        })
        .catch((err) => {
          console.log(err);
        });
    }
  };
</script>

<div>
  <form on:submit|preventDefault={handleSubmit}>
    <h3>Secure your page</h3>
    <br />
    <Warning show={!!$error}>
      <span slot="phrase">⛔️ {$error} ⛔️</span>
    </Warning>

    <div>
      <input
        bind:value={$username}
        type="text"
        variant="outlined"
        label="Username"
        aria-controls="super-helper-Username"
        aria-describedby="super-helper-Username"
        autocomplete="username"
        disabled
        id="username"
      />
      <label for="username" id="super-helper-Username">
        This will be your @username
      </label>
    </div>

    <div>
      <input
        bind:value={$password}
        type="password"
        variant="outlined"
        label="Passphrase"
        aria-controls="super-helper"
        aria-describedby="super-helper"
        on:keydown={handlePasswordInput}
        on:keyup={handlePasswordInput}
        on:keydown={handleInputKeyPress}
        autocomplete="new-password"
        id="password"
      />
      <label for="" id="super-helper">
        Easy to remember phrase like "I grew up on Main Street back in 87"
      </label>

      <InvalidWarning {validation} />
    </div>

    <h3>Type passphrase again to to confirm:</h3>
    <br />
    <input
      bind:value={confirmValue}
      type="password"
      variant="outlined"
      label="Confirm Passphrase"
      aria-controls="super-helper-2"
      aria-describedby="super-helper-2"
      on:keypress={handleInputKeyPress}
      on:keyup={checkMatch}
      on:keydown={checkMatch}
      on:blur={checkMatch}
      on:focus={checkMatch}
      id="password-confirm"
    />
    <label for="" id="super-helper-2">Press Enter when you're ready!</label>

    <Warning show={!match}>
      <span slot="phrase">⛔️ Passphrases don't match ⛔️</span>
    </Warning>

    <div>
      <div class="FormField">
        <input type="checkbox" bind:group={agreements} value="remember" />
        <span class="label">Remember me</span>
      </div>
      <br clear="all" />
      <div class="FormField">
        <input type="checkbox" bind:group={agreements} value="checkedTerms" />
        <span class="label">I agree to the terms and conditions of use</span>
      </div>
    </div>

    <div>
      <button
        on:click={() => ($lockerSection = "LogInOrCreateChoice")}
        variant="outlined">
        <span class="material-icons">backspace</span>
        Cancel
      </button>
      <button class="raised" on:click={handleSubmit}>
        <span>Next</span>
      </button>
    </div>
  </form>
</div>

<style>
  div {
    margin-top: 1em;
    margin-bottom: 1em;
  }
</style>
