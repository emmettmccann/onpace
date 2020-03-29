<script>
  import TimeDisp from "./TimeDisp.svelte";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();
  export let lapTimes = [];
  export let lapCount;
  export let goalTime;
  $: remainingLaps = lapCount - lapTimes.length;
  $: currentPace = lapTimes[lapTimes.length - 1];
  $: ellapsedTime = lapTimes.reduce((total, el) => {
    return total + el;
  }, 0);
  $: averagePace = ellapsedTime / lapTimes.length;
  $: totalTime = ellapsedTime + remainingLaps * currentPace;
  $: projDifference = totalTime - goalTime;
  $: mustAverage = updateMustAverage(goalTime, ellapsedTime, remainingLaps);
  $: distanceCovered = lapTimes.length * 50;
  $: finished = remainingLaps == 0;

  function updateMustAverage(gt, et, rl) {
    let val = (gt - et) / rl;
    dispatch("mustHold", {
      time: val,
      delta: projDifference
    });
    return val;
  }
</script>

<style>
  #tooslow,
  #onpace {
    font-size: 3rem;
    font-weight: 400;
    line-height: 3rem;
    @apply my-10;
  }

  #main {
    @apply py-8 flex flex-col items-center bg-blue-100 m-3;
  }
</style>

<div id="main">
  <div class="flex flex-row justify-center w-full text-2xl text-right">
    {#if !finished}
      <div class="mx-2">
        <div>Must Average:</div>
        <div>Last split:</div>
        <div>Estimated Time:</div>
        <!-- <div>Projected Delta:</div> -->
      </div>

      <div class="flex flex-col items-end flex-grow-0 mx-3">
        <TimeDisp time={mustAverage} />
        <TimeDisp time={currentPace} />
        <TimeDisp time={totalTime} />
        <!-- <TimeDisp split fullTime time={projDifference} /> -->
      </div>
    {:else}
      <div class="mx-2">
        <div>Average Pace:</div>
        <div>Last split:</div>
        <div>Total Time:</div>
      </div>

      <div class="flex flex-col items-end flex-grow-0 mx-3">
        <TimeDisp time={averagePace} />
        <TimeDisp time={currentPace} />
        <TimeDisp time={totalTime} />
      </div>
    {/if}
  </div>
</div>
<!-- 
{#if lapTimes.length > 0}
  {#if projDifference < 0}
    <div id="onpace" class="text-green-500">On Pace!</div>
  {:else}
    <div id="tooslow" class="text-red-600">Too Slow!</div>
  {/if}
{/if} -->
