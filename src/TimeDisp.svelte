<script>
  export let time = 0;
  $: seconds = checkTime(Math.floor(Math.abs(time) / 1000) % 60);
  $: fracs = checkTime(Math.floor((Math.abs(time) % 1000) / 10));
  $: minutes = checkTime(Math.floor(Math.abs(time) / 60000));

  $: displayTime =
    (time < 0 ? "-" : "") + minutes + ":" + seconds + "." + fracs;

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

{displayTime}
