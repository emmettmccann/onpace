<script>
  import TimeDisp from "./TimeDisp.svelte";

  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  let currentTime = 0.0;
  let startTime = 0.0;
  let running = false;
  let lapTimes = [];
  let splitTimes = [];
  let timer;

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
</style>

<div id="time">
  <TimeDisp time={currentTime} />
</div>
{#if !running}
  <button on:click={startTimer}>Start</button>
  <button on:click={resetTimer}>Reset</button>
{:else}
  <button on:click={stopTimer}>Stop</button>
  <button on:click={addLapTime}>Lap</button>
{/if}
