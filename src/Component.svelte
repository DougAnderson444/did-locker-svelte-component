<script>
  import didlocker from "did-locker"; // uses the browser field in package.json
  // import { lockerSections } from "./components/LockerSections";
  import Timer from "./components/LockerSections/Timer.svelte";

  // //svelte stores
  import { lockerSection, dnsLink, wallet } from "./js/stores.js";
  import { onMount } from "svelte";

  // Properties (Props) pass into this component from the parent
  export let hypnsNode;
  export let options;

  // let active;

  $: {
    if (!$wallet && hypnsNode) {
      const options = { methods: { hypnsNode } };
      didlocker(options).then((w) => {
        $wallet = w;
      });
    }
  }
  // $: {
  //   if (
  //     $wallet &&
  //     $lockerSection != "CreateNewUser" &&
  //     $lockerSection != "SetupDevice" &&
  //     $lockerSection != "CreateUserProgress"
  //   ) {
  //     if ($wallet.locker.isPristine()) {
  //       console.log(`wallet isPristine`);
  //       if (!$dnsLink) $lockerSection = "CreateNewUser";
  //       else $lockerSection = "LockScreen";
  //     }
  //   }
  // }

  // const handleLockedChanged = () => {
  //   $wallet = $wallet; // triggers reactivity that depends on a refreshed $wallet, like Timer
  // };

  // // Anything with $: in front means it's reactive (built in to svelte)
  // $: if ($wallet && $wallet.locker.isLocked()) $lockerSection = "LockScreen";
  // $: $wallet ? $wallet.locker.onLockedChange(handleLockedChanged) : null; // lock and unlock signal
  // $: active = lockerSections[$lockerSection];

  onMount(async () => {});
</script>

<svelte:head>
  <title>Your Data Syncs</title>
</svelte:head>

<div class="content">
  Locker
  <!-- {#if active && $lockerSection}
    <svelte:component this={active.component} {hypnsNode} {options} />
  {/if} -->
</div>
<svelte:component this={Timer} />
