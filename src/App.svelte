<script>
  import Timer from "./Timer.svelte";
  import Race from "./Race.svelte";
  import TimeDisp from "./TimeDisp.svelte";
  import TimeInput from "./TimeInput.svelte";
  let lapCount;
  let goalTime;
  let lapTimes;

  function updateLapTimes(event) {
    lapTimes = event.detail.lapTimes;
  }
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
  <form action="newRace">
    <input type="number" id="distance" bind:value={lapCount} />
    <input type="number" id="goalTime" bind:value={goalTime} />
    <div>
      <TimeInput enteredTime={goalTime} />
    </div>
  </form>

  <Timer on:lapTimes={updateLapTimes} />
  <Race {lapTimes} {lapCount} {goalTime} />
</main>
