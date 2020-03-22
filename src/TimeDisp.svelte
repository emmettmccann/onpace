<script>
  export let time = 0;
  export let split = false;
  export let fullTime = false;
  $: seconds = checkTime(Math.floor(Math.abs(time) / 1000) % 60);
  $: fracs = checkTime(Math.floor((Math.abs(time) % 1000) / 10));
  $: minutes = checkTime(Math.floor(Math.abs(time) / 60000));

  $: dispMinSec =
    (time < 0 ? "-" : " ") + minutes + ":" + seconds + "." + fracs;

  $: dispSec = (time < 0 ? "-" : " ") + seconds + "." + fracs;

  $: displayTime = isNaN(time)
    ? split && !fullTime
      ? "--.--"
      : "--:--.--"
    : split && !fullTime
    ? dispSec
    : dispMinSec;

  function checkTime(i) {
    if (i < 10) {
      i = "0" + i;
    } // add zero in front of numbers < 10
    return i;
  }
</script>

<style>
  /* your styles go here */
</style>

{#if !split}
  <!-- Non-split, normal time -->
  <span>{displayTime}</span>
{:else if time > 0}
  <!-- positive split = too slow = red -->
  <span class="text-red-600">{displayTime}</span>
{:else}
  <!-- negative time = too fast = green -->
  <span class="text-green-500">{displayTime}</span>
{/if}
