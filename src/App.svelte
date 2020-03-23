<script>
  import Timer from "./Timer.svelte";
  import Race from "./Race.svelte";
  import TimeDisp from "./TimeDisp.svelte";
  import TimeInput from "./TimeInput.svelte";
  let lapCount;
  let goalTime;
  let lapTimes;
  let mustHold;

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
  }
</script>

<style global>
  @tailwind base;
  @tailwind components;
  @tailwind utilities;

  #main-app {
    @apply p-3 rounded-lg m-0 bg-white h-full;
  }

  #page {
    @apply bg-green-300 p-3 h-screen;
  }
</style>

<main id="page">
  <div id="main-app">
    <form
      on:submit|preventDefault
      class="flex flex-row justify-around w-full text-xl">
      <select bind:value={lapCount} on:change={console.log(lapCount)}>
        {#each distanceList as distance}
          <option value={distance.laps}>{distance.distance}</option>
        {/each}
      </select>
      <TimeInput on:valueInMs={updateGoalTime} />
    </form>

    <Race {lapTimes} {lapCount} {goalTime} on:mustHold={updateMustHold} />
    <Timer on:lapTimes={updateLapTimes} {mustHold} {lapCount} />

  </div>
</main>
