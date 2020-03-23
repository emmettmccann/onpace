<script>
  import Timer from "./Timer.svelte";
  import Race from "./Race.svelte";
  import TimeDisp from "./TimeDisp.svelte";
  import TimeInput from "./TimeInput.svelte";
  import PaceAlert from "./PaceAlert.svelte";
  let lapCount;
  let goalTime;
  let lapTimes;
  let mustHold;
  let delta;

  let distanceList = [
    { id: 1, distance: "500yd", laps: 10 },
    { id: 2, distance: "1000yd", laps: 20 },
    { id: 3, distance: "1650yd", laps: 33 },
    { id: 4, distance: "400m", laps: 8 },
    { id: 5, distance: "800m", laps: 16 },
    { id: 6, distance: "1500m", laps: 30 }
  ];

  function updateLapTimes(event) {
    lapTimes = event.detail.lapTimes;
  }

  function updateGoalTime(event) {
    goalTime = event.detail.time;
  }

  function updateMustHold(event) {
    mustHold = event.detail.time;
    delta = event.detail.delta;
  }
</script>

<style global>
  @tailwind base;
  @tailwind components;
  @tailwind utilities;

  #main-app {
    @apply p-3 pt-5 m-0 bg-white h-full;
  }
</style>

<div id="main-app">
  <form
    on:submit|preventDefault
    class="flex flex-row justify-between w-full text-xl">
    <select bind:value={lapCount} on:change={console.log(lapCount)}>
      {#each distanceList as distance}
        <option value={distance.laps}>{distance.distance}</option>
      {/each}
    </select>
    <TimeInput on:valueInMs={updateGoalTime} />
  </form>

  <div class="flex flex-col h-full">
    <div>
      <Race {lapTimes} {lapCount} {goalTime} on:mustHold={updateMustHold} />
    </div>
    <div class="flex-grow">
      <PaceAlert {delta} {lapTimes} />
    </div>
    <div>
      <Timer on:lapTimes={updateLapTimes} {mustHold} {lapCount} />
    </div>
  </div>
</div>
