<script>
  import TimeDisp from "./TimeDisp.svelte";
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
  $: mustAverage = goalTime - ellapsedTime;
  $: distanceCovered = lapTimes.length * 50;
</script>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<main>
  <div>
    Total Estimated Time:
    <TimeDisp time={totalTime} />
    Goal Time:
    <TimeDisp time={goalTime} />
    Projected Difference:
    <TimeDisp time={projDifference} />
    Must Average:
    <TimeDisp time={mustAverage} />
    Most recent split:
    <TimeDisp time={currentPace} />
    Distance Covered: {distanceCovered}
  </div>
  <p />
  <div>
    {#each lapTimes as time}
      <TimeDisp {time} />
    {/each}
  </div>
</main>
