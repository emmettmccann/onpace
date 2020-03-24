<script>
  import TimeInput from "./TimeInput.svelte";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  let goalTime;
  export let goalTimeFormatted;
  export let lapCount;
  let distanceList = [
    { id: 1, distance: "500yd", laps: 10 },
    { id: 2, distance: "1000yd", laps: 20 },
    { id: 3, distance: "1650yd", laps: 33 },
    { id: 4, distance: "400m", laps: 8 },
    { id: 5, distance: "800m", laps: 16 },
    { id: 6, distance: "1500m", laps: 30 }
  ];

  function updateGoalTime(event) {
    goalTime = event.detail.time;
  }

  let enteredTime = goalTimeFormatted;

  let minutes, seconds, fracs, ms;

  function validate(event) {
    let digitList = event.target.value.match(/[0-9]/g);
    let digits = digitList.reduce((tot, el) => {
      return tot + el;
    }, "000000");
    digits = digits.slice(-6);
    console.log(digitList);
    minutes = digits.slice(-6, -4);
    seconds = digits.slice(-4, -2);
    fracs = digits.slice(-2);
    ms = 0;
    let disp = "";
    if (digits.length > 4) {
      disp += minutes + ":";
      ms += parseInt(minutes) * 6000;
    }
    if (digits.length > 2) {
      disp += seconds + ".";
      ms += parseInt(seconds) * 100;
    }
    disp += fracs;
    ms += parseInt(fracs);
    goalTime = ms * 10;
    enteredTime = disp;
  }

  function updateRaceSettings(event) {
    console.log("settings");
    dispatch("newSettings", {
      goalTime: goalTime,
      lapCount: lapCount
    });
  }

  function closeSettings(event) {
    dispatch("closed", {});
  }
</script>

<style>
  /* your styles go here */
</style>

<div
  class="fixed top-0 left-0 z-50 flex w-screen h-screen overflow-auto bg-smoke">
  <div
    class="relative flex flex-col w-full max-w-md p-8 m-auto bg-white rounded-md">
    <form
      on:submit|preventDefault={updateRaceSettings}
      class="flex flex-col items-center justify-between w-full text-xl">
      <label class="text-3xl" for="distance">Race Distance</label>
      <select
        id="distance"
        class="text-2xl"
        bind:value={lapCount}
        on:change={console.log(lapCount)}>
        {#each distanceList as distance}
          <option value={distance.laps}>{distance.distance}</option>
        {/each}
      </select>
      <label class="pt-8 text-3xl" for="input-time">Goal Time</label>
      <input
        id="input-time"
        class="text-2xl"
        type="text"
        bind:value={enteredTime}
        name="time"
        on:input={validate}
        on:submit|preventDefault={updateRaceSettings} />
      <div class="pt-8">
        <button
          type="button"
          class="p-3 m-3 text-xl font-bold text-white bg-red-500 border-none rounded-md shadow-md"
          on:click|preventDefault={closeSettings}>
          Cancel
        </button>
        <button
          type="submit"
          class="p-3 m-3 text-xl font-bold text-white bg-green-500 border-none rounded-md shadow-md"
          on:click|preventDefault={updateRaceSettings}>
          Save Settings
        </button>
      </div>
    </form>
  </div>
  <span class="absolute top-0 right-0 p-4" on:click={closeSettings}>
    <svg
      class="w-12 h-12 text-red-500 fill-current"
      role="button"
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 20 20">
      <title>Close</title>
      <path
        d="M14.348 14.849a1.2 1.2 0 0 1-1.697 0L10 11.819l-2.651 3.029a1.2 1.2 0
        1 1-1.697-1.697l2.758-3.15-2.759-3.152a1.2 1.2 0 1 1 1.697-1.697L10
        8.183l2.651-3.031a1.2 1.2 0 1 1 1.697 1.697l-2.758 3.152 2.758 3.15a1.2
        1.2 0 0 1 0 1.698z" />
    </svg>
  </span>
</div>
