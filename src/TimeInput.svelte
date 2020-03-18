<script>
  import TimeDisp from "./TimeDisp.svelte";

  export let enteredTime;

  $: seconds = Math.floor((enteredTime % 10000) / 100);
  $: fracs = Math.floor(enteredTime % 100);
  $: minutes = Math.floor((enteredTime % 1000000) / 10000);
  $: ms = minutes * 6000 + seconds * 100 + fracs;
  $: displayTime = minutes + ":" + seconds + "." + fracs;
</script>

<style>
  /* your styles go here */
</style>

<div>{displayTime}</div>
