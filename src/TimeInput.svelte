<script>
  import TimeDisp from "./TimeDisp.svelte";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  let enteredTime = "00:00.00";

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
    dispatch("valueInMs", {
      time: ms * 10
    });
    enteredTime = disp;
  }
</script>

<style>
  /* Chrome, Safari, Edge, Opera */
  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
  }

  /* Firefox */
  input[type="number"] {
    -moz-appearance: textfield;
  }
</style>

<input
  id="input-time"
  type="phone"
  bind:value={enteredTime}
  name="time"
  on:input={validate} />
