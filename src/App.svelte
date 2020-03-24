<script>
  import Timer from "./Timer.svelte";
  import Race from "./Race.svelte";
  import TimeDisp from "./TimeDisp.svelte";
  import PaceAlert from "./PaceAlert.svelte";
  import SettingsModal from "./SettingsModal.svelte";
  let lapCount;
  let goalTime = 0.0;
  let lapTimes;
  let mustHold;
  let delta;
  let goalTimeFormatted = "00:00.00";
  let settings = true;

  function updateLapTimes(event) {
    lapTimes = event.detail.lapTimes;
  }

  function updateMustHold(event) {
    mustHold = event.detail.time;
    delta = event.detail.delta;
  }

  function updateSettings(event) {
    goalTime = event.detail.goalTime;
    lapCount = event.detail.lapCount;
    settings = false;
  }

  function closeSettings(event) {
    settings = false;
  }

  function updateFormattedGoal(event) {
    goalTimeFormatted = event.detail.time;
    console.log("gtf" + goalTimeFormatted);
  }
</script>

<style global>
  @tailwind base;
  @tailwind components;
  @tailwind utilities;

  #main-app {
    @apply p-3 m-0 bg-white h-full;
  }
</style>

<div id="main-app">
  {#if settings}
    <SettingsModal
      {goalTimeFormatted}
      {lapCount}
      on:newSettings={updateSettings}
      on:closed={closeSettings} />
  {/if}

  <button
    on:click={() => {
      settings = true;
    }}>
    settings
  </button>

  <TimeDisp returnTime on:formattedTime={updateFormattedGoal} time={goalTime} />
  {lapCount}
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
