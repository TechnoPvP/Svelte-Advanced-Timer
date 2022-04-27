<script lang="ts">
  import { onDestroy, onMount } from 'svelte';
  import { flip } from 'svelte/animate';
  import SlotButton from './SlotButton.svelte';

  interface Clock {
    days?: number;
    hours?: number;
    seconds?: number;
    minutes?: number;
  }

  export let timeInput: string = '';

  let clockTime: Date;
  let interval: NodeJS.Timer | null;

  $: parseTimeString(timeInput);

  let clock: Clock = {};

  /**
   * Parse a time string into a hour/minute/second number format
   *
   * @param time
   * @example "1h 10m 30s" = [1, 10, 30]
   */
  function parseTimeString(time: string): void {
    let hoursGroup = time.match(/([\d.]+) *h/);
    let minuteGroup = time.match(/([\d.]+) *m/);
    let daysGroup = time.match(/([\d.]+) *d/);
    let secondsGroup = time.match(/([\d.]+) *s/);

    clock.days = daysGroup?.length ? parseInt(daysGroup[1]) : undefined;
    clock.hours = hoursGroup?.length ? parseInt(hoursGroup[1]) : undefined;
    clock.minutes = minuteGroup?.length ? parseInt(minuteGroup[1]) : undefined;
    clock.seconds = secondsGroup?.length ? parseInt(secondsGroup[1]) : undefined;

    // Default to minutes if no date format is available
    if (timeInput && !hoursGroup && !minuteGroup && !daysGroup && !secondsGroup) {
      const minutes = parseInt(timeInput);
      setCountdownDate(getTimerDate({ minutes }));
    }
  }

  /**
   * Return a date from Clock values.
   *
   * @param date
   */
  function getTimerDate(date: Clock) {
    const currentDate = new Date();
    const timerDate = new Date();

    timerDate.setDate(timerDate.getDate() + (date?.days || 0));
    timerDate.setHours(currentDate.getHours() + (date?.hours || 0));
    timerDate.setMinutes(currentDate.getMinutes() + (date?.minutes || 0));
    timerDate.setSeconds(currentDate.getSeconds() + (date?.seconds || 0));

    return timerDate;
  }

  /**
   * Update the clock timer to reflect the current countdown.
   * @param timerDate
   */
  function setCountdownDate(timerDate: Date) {
    const currentDate = new Date();

    let timeDifference = Math.abs(currentDate.valueOf() - timerDate.valueOf());
    let days = timeDifference / (24 * 60 * 60 * 1000);
    let hours = (days % 1) * 24;
    let minutes = (hours % 1) * 60;
    let seconds = (minutes % 1) * 60;
    [days, hours, minutes, seconds] = [Math.floor(days), Math.floor(hours), Math.floor(minutes), Math.floor(seconds)];

    clock.days = days || undefined;
    clock.hours = hours || undefined;
    clock.minutes = minutes || undefined;
    clock.seconds = seconds || undefined;
  }

  function startCountdown() {
    if (interval) return;
    clockTime = getTimerDate(clock);

    interval = setInterval(() => {
      clockTime.setSeconds(clockTime.getSeconds());
      setCountdownDate(clockTime);
    }, 1000);
  }

  const handleInput = () => stopCountdown();

  const handleReset = () => {
    timeInput = '';
    stopCountdown();
  };

  const stopCountdown = () => {
    if (interval) clearInterval(interval);
    interval = null;
  };

  onMount(() => {});

  onDestroy(() => {
    if (interval) clearInterval(interval);
  });
</script>

<section>
  <article>
    <h4>How It works</h4>
    <p>The intuitive countdown timer will parse and detect your countdown.</p>
    <p>Example 2: 200d 1m</p>
  </article>

  <header>
    <h4>Insert time</h4>
    <input on:input={handleInput} bind:value={timeInput} placeholder="1d 10h 20m 30s" type="text" name="" id="" />
  </header>

  <div class="actions">
    <SlotButton on:click={startCountdown}>Start</SlotButton>
    <SlotButton on:click={stopCountdown}>Pause</SlotButton>
    <SlotButton on:click={handleReset}>Reset</SlotButton>
  </div>

  <div class="clock">
    {#each Object.entries(clock) as [key, value] (key)}
      <div class="clock__countdown" animate:flip={{ duration: 250 }}>
        {#if value != undefined}
          <h3>{value}{key.substring(0, 1)}</h3>
        {/if}
      </div>
    {/each}
  </div>
</section>

<style lang="scss">
  header {
    display: flex;
    flex-direction: column;

    h4 {
      margin: 0;
      margin-bottom: 5px;
    }
    // gap: 1rem;
  }
  article {
    display: flex;
    flex-direction: column;
    gap: 12px;
    margin-top: -40px;
    margin-bottom: 3rem;
    p {
      margin: 0;
    }

    h4 {
      margin: 0;
    }
  }
  .clock {
    display: flex;
    justify-content: center;
    font-size: 40px;
    gap: 1.3rem;

    h3 {
      display: flex;
      justify-content: center;
      min-width: 110px;
    }
  }
  section {
    display: flex;
    justify-content: center;
    // align-items: center;
    flex-direction: column;
    gap: 1rem;

    p {
      max-width: 35ch;
      font-size: 14px;
      opacity: 0.8;
      line-height: 1.5;
    }
  }
  .actions {
    display: flex;
    gap: 1rem;
  }
  input {
    padding: 8px;
    border-radius: 7px;
  }
</style>
