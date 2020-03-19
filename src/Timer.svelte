<script>
  import TimeDisp from "./TimeDisp.svelte";

  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let lapCount = 10;

  let currentTime = 0.0;
  let startTime = 0.0;
  let running = false;
  let lapTimes = [];
  let splitTimes = [];
  let timer;

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
    let split =
      lapTimes.length > 0
        ? currentTime - lapTimes[lapTimes.length - 1]
        : currentTime;
    lapTimes = lapTimes.concat(currentTime);
    splitTimes = splitTimes.concat(split);
    dispatch("lapTimes", {
      lapTimes: splitTimes
    });
  }
</script>

<style>
  #time {
    font-family: "Courier New", Courier, monospace;
    font-size: 5rem;
  }

  #split {
    font-family: "Courier New", Courier, monospace;
    font-size: 2rem;
  }
  button {
    width: 100%;
    height: 5rem;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
      Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
    font-size: 3rem;
  }
</style>

<div id="split">
  <TimeDisp time={splitTimes[splitTimes.length - 1]} />
</div>
<div id="time">
  <TimeDisp time={currentTime} />
</div>

{#if !running & (lapTimes.length == 0)}
  <button on:click={startTimer}>Start</button>
{:else if running & (lapTimes.length < lapCount - 1)}
  <button on:click={addLapTime}>{distanceCovered + 50} split</button>
{:else if running & (lapTimes.length == lapCount - 1)}
  <button on:click={stopTimer}>Finish</button>
{:else if lapTimes.length == lapCount}
  <button on:click={resetTimer}>Reset</button>
{/if}
