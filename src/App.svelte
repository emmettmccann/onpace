<script>
  import Timer from "./Timer.svelte";
  import Race from "./Race.svelte";
  import TimeDisp from "./TimeDisp.svelte";
  import Time2 from "./TimeInput.svelte";
  let lapCount;
  let goalTime;
  let lapTimes;

  let distanceList = [
    { id: 1, distance: "500", laps: 10 },
    { id: 2, distance: "1000", laps: 20 },
    { id: 3, distance: "1650", laps: 33 },
    { id: 4, distance: "400", laps: 8 },
    { id: 5, distance: "800", laps: 16 },
    { id: 6, distance: "1500", laps: 30 }
  ];

  function updateLapTimes(event) {
    lapTimes = event.detail.lapTimes;
  }

  function updateGoalTime(event) {
    goalTime = event.detail.time;
  }
</script>

<style global>
  @tailwind base;
  @tailwind components;
  @tailwind utilities;
</style>

<!-- <style>
  main {
    text-align: center;
    padding: 1em;
    max-width: none;
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
      max-width: 33%;
    }
  }
</style> -->
<main>
  <form action="newRace">
    <select bind:value={lapCount} on:change={console.log(lapCount)}>
      {#each distanceList as distance}
        <option value={distance.laps}>{distance.distance}</option>
      {/each}
    </select>
    <Time2 on:valueInMs={updateGoalTime} />
  </form>

  <Race {lapTimes} {lapCount} {goalTime} />
  <Timer on:lapTimes={updateLapTimes} />

</main>
