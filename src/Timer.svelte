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
</script>

<style>
  #timer {
    touch-action: manipulation;
    @apply flex flex-col items-center w-full mt-5 pb-10;
  }

  button {
    @apply font-bold text-3xl font-sans w-11/12 h-20 py-2 px-4 rounded text-white;
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
    <button class="bg-green-500 active:bg-green-400" on:click={startTimer}>
      Start
    </button>
  {:else if running & (lapTimes.length < lapCount - 1)}
    <button class="bg-blue-500 active:bg-blue-400" on:click={addLapTime}>
      {distanceCovered + 50} split
    </button>
  {:else if running & (lapTimes.length == lapCount - 1)}
    <button class="bg-orange-500 active:bg-orange-400" on:click={stopTimer}>
      Finish
    </button>
  {:else if lapTimes.length == lapCount}
    <button class="bg-red-500 active:bg-red-400" on:click={resetTimer}>
      Reset
    </button>
  {/if}
  {#if running}
    <button class="bg-orange-500 active:bg-orange-400" on:click={stopTimer}>
      Stop
    </button>
  {:else}
    <button class="bg-red-500 active:bg-red-400" on:click={resetTimer}>
      Reset
    </button>
  {/if}
</div>
