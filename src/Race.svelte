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
  $: totalTime = ellapsedTime + remainingLaps * currentPace;
  $: projDifference = totalTime - goalTime;
  $: mustAverage = updateMustAverage(goalTime, ellapsedTime, remainingLaps);
  $: distanceCovered = lapTimes.length * 50;

  function updateMustAverage(gt, et, rl) {
    let val = (gt - et) / rl;
    dispatch("mustHold", {
      time: val
    });
    return val;
  }
</script>

<style>
  #tooslow,
  #onpace {
    font-size: 3rem;
    font-weight: 400;
    line-height: 6rem;
  }

  #main {
    @apply p-8 flex flex-col items-center bg-blue-100 m-3;
  }
</style>

<div id="main">
  <div>
    <div class="flex flex-row text-2xl text-right items-between width-full">
      <div class="m-2">
        <div>Must Average:</div>
        <div>Total Estimated Time:</div>
        <div>Projected Difference:</div>
        <div>Most recent split:</div>
      </div>
      <div class="flex flex-col m-2">
        <TimeDisp time={mustAverage} />
        <TimeDisp time={totalTime} />
        <TimeDisp split fullTime time={projDifference} />
        <TimeDisp time={currentPace} />
      </div>
    </div>

    {#if lapTimes.length > 0}
      {#if projDifference < 0}
        <div id="onpace" class="text-green-500">On Pace!</div>
      {:else}
        <div id="tooslow" class="text-red-600">Too Slow!</div>
      {/if}
    {/if}

    <!-- <div>Distance Covered: {distanceCovered}</div> -->
  </div>
  <p />
  <!-- <div>
    {#each lapTimes as time}
      <TimeDisp {time} />
    {/each}
  </div> -->
</div>
