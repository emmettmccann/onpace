<script>
  import TimeDisp from "./TimeDisp.svelte";

  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let lapCount = 10;
  export let mustHold = 0.0;

  let currentTime = 0.0;
  let startTime = 0.0;
  let running = false;
  let lapTimes = [];
  let splitTimes = [];
  let timer;
  let splitOn = false;
  $: mostRecentSplit =
    splitTimes.length > 0 ? splitTimes[splitTimes.length - 1] : 0.0;

  $: splitDelta = mostRecentSplit - mustHold;

  $: split =
    lapTimes.length > 0
      ? currentTime - lapTimes[lapTimes.length - 1]
      : currentTime;

  $: distanceCovered = lapTimes.length * 50;

  function startTimer(event) {
    // only set the startTime if the clock is currently reset.
    if (startTime == 0.0) {
      startTime = Date.now();
    } else {
      startTime = Date.now() - currentTime;
    }
    timer = setInterval(() => {
      currentTime = Date.now() - startTime;
    }, 10);
    running = true;
  }

  function stopTimer(event) {
    addLapTime(event);
    clearInterval(timer);
    running = false;
  }

  function resetTimer(event) {
    clearInterval(timer);
    currentTime = 0.0;
    startTime = 0.0;
    lapTimes = [];
    splitTimes = [];
    dispatch("lapTimes", {
      lapTimes: splitTimes
    });
  }

  function addLapTime(event) {
    if (splitOn) return;
    lapTimes = lapTimes.concat(currentTime);
    splitTimes = splitTimes.concat(split);
    dispatch("lapTimes", {
      lapTimes: splitTimes
    });
    splitOn = true;
    setTimeout(() => {
      splitOn = false;
    }, 2000);
  }

  function openSettings(event) {
    dispatch("openSettings", {});
  }
</script>

<style>
  #timer {
    touch-action: manipulation;
    @apply flex flex-col items-center w-full mt-5 pb-10;
  }

  button {
    @apply font-bold text-3xl font-sans w-11/12 h-20 py-2 px-4 my-2 rounded text-white;
  }

  .start {
    @apply bg-green-500;
  }
  .split {
    @apply bg-blue-500;
  }
  .finish {
    @apply bg-orange-400 h-40;
  }
  .reset,
  .stop {
    @apply bg-red-500;
  }
  .settings {
    @apply bg-purple-500;
  }
</style>

<div id="timer">
  <div id="split" class="font-mono text-3xl font-medium">
    {#if splitOn}
      <!-- show paused split time -->
      <TimeDisp split time={splitDelta} />
    {:else}
      <!-- show running split time -->
      <TimeDisp time={split} />
    {/if}
  </div>
  <div id="time" class="font-mono text-6xl font-light">
    {#if splitOn}
      <!-- splitTime -->
      <TimeDisp time={mostRecentSplit} />
    {:else}
      <!-- show running clock -->
      <TimeDisp time={currentTime} />
    {/if}
  </div>

  {#if !running & (lapTimes.length == 0)}
    <!-- startsettings -->
    <button class="start" on:click={startTimer}>Start</button>
    <button class="settings" on:click={openSettings}>Race Settings</button>
  {:else if running & (lapTimes.length < lapCount - 1)}
    <button class="split" on:click={addLapTime}>
      {distanceCovered + 50} split
    </button>
    <div id="two-button" class="flex flex-row w-11/12">
      <button class="mr-2 stop" on:click={stopTimer}>Stop</button>
      <button class="ml-2 settings" on:click={openSettings}>Settings</button>
    </div>
  {:else if running & (lapTimes.length == lapCount - 1)}
    <button class="finish" on:click={stopTimer}>Finish</button>
  {:else}
    <button class="reset" on:click={resetTimer}>Reset</button>
    <button class="settings" on:click={openSettings}>Settings</button>
  {/if}
</div>

<!-- 
  pre-race startsettings
  during lapstopsettings
  armed for finish finish
  finished/stopped resetsettings
 -->
