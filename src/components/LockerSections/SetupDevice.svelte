<script>
  import {
    lockerSection,
    deviceType,
    deviceName,
    name,
    ipfsNode,
    wallet,
    password,
  } from "../../js/stores.js";
  import { onMount } from "svelte";

  import { DEVICE_TYPES } from "../../js/constants";
  const LOCK_TYPE = "passphrase";
  $deviceName = "My Samsung";
  $deviceType = DEVICE_TYPES[0];
  $name = "Identity Name";
  let selectedDeviceType = $deviceType;
  let disabled = true;
  $: $ipfsNode ? (disabled = false) : (disabled = true);

  const handleButtonClick = async () => {
    $lockerSection = "CreateUserProgress";
  };
</script>

<h3>Name/Nickname this identity (so you can tell them apart)</h3>
<div class="vert">
  <input
    type="text"
    bind:value={$name}
    on:focus|once={() => {
      $name = "";
    }}
    variant="outlined"
    label="Identity Name"
    aria-controls="helper-text-outlined-device"
    aria-describedby="helper-text-outlined-device"
    autocomplete="nickname"
    id="nickname"
    name="nickname"
  />
  <label for="nickname" id="helper-text-outlined-device"
    >Doug, Acme Law Office, etc.</label
  >
</div>

<h3>Type of device (it's smart to add a few devices)</h3>
<div class="narrow vert">
  <div class="list">
    {#each DEVICE_TYPES as device}
      <span>{device}</span><input
        type="radio"
        bind:group={selectedDeviceType}
        value={device}
      />
    {/each}
  </div>
</div>

<h3>Name this device (it's smart to add a few devices)</h3>
<div class="vert">
  <input
    type="text"
    bind:value={$deviceName}
    on:focus|once={() => {
      $deviceName = "";
    }}
    variant="outlined"
    label="Device Nickname"
    aria-controls="helper-text-outlined-device"
    aria-describedby="helper-text-outlined-device"
    autocomplete="devicenickname"
    id="devicenickname"
    name="devicenickname"
  />
  <label for="devicenickname" id="helper-text-outlined-device"
    >Device Nickname</label
  >
</div>

<button class="raised" on:click={handleButtonClick} {disabled}> Next </button>
{#if disabled}
  <br />
  Just a moment while your browser finishes creating your Web3 space
{/if}

<style>
  * :global(.list) {
    max-width: 600px;
    border: 1px solid rgba(0, 0, 0, 0.15);
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    border-radius: 5px;
  }
  .narrow {
    width: 250px;
  }
  .vert {
    margin: 1em 0 1em 0;
    padding: 1em 0 1em 0;
  }
</style>
