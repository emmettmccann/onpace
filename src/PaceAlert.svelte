<script>
  export let lapTimes = [];
  export let delta = 0;
  export let timeState;
</script>

<style>
  #tooslow,
  #onpace {
    font-size: 3rem;
    font-weight: 400;
  }
</style>

<div class="flex flex-col justify-center w-full h-full text-center">
  {#if lapTimes.length > 0}
    {#if delta < 0}
      <div id="onpace" class="text-green-500">On Pace!</div>
    {:else}
      <div id="tooslow" class="text-red-600">Too Slow!</div>
    {/if}
  {:else if timeState}
    <div id="onpace" class="text-green-500">Running</div>
  {:else}
    <div id="onpace" class="text-green-500">Ready to Start!</div>
  {/if}

</div>
