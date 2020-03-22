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
  #onpace {
    color: lime;
  }
  #tooslow {
    color: red;
  }

  #tooslow,
  #onpace {
    font-size: 3rem;
    font-weight: 400;
    line-height: 6rem;
  }
</style>

<div>
  <div>
    <div>
      Goal Time:
      <TimeDisp time={goalTime} />
    </div>
    <div>
      Must Average:
      <TimeDisp time={mustAverage} />
    </div>
    <div>
      Total Estimated Time:
      <TimeDisp time={totalTime} />
    </div>

    <div>
      Projected Difference:
      <TimeDisp time={projDifference} />
    </div>

    {#if lapTimes.length > 0}
      {#if projDifference < 0}
        <div id="onpace">On Pace!</div>
      {:else}
        <div id="tooslow">Too Slow!</div>
      {/if}
    {/if}

    <div>
      Most recent split:
      <TimeDisp time={currentPace} />
    </div>
    <!-- <div>Distance Covered: {distanceCovered}</div> -->
  </div>
  <p />
  <!-- <div>
    {#each lapTimes as time}
      <TimeDisp {time} />
    {/each}
  </div> -->
</div>
