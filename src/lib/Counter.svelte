<script lang="ts">
  import { tweened } from 'svelte/motion';
 
  export let intitalCount = 0;
  let initalCount = tweened(intitalCount, { duration: 0 });

</script>

<section>
  <div class="counter">
    <button class="counter__button counter__button--dec" on:click={() => ($initalCount ? $initalCount-- : null)}>
      <span>-</span>
    </button>
    <span class="counter__count">{$initalCount.toFixed(0)}</span>
    <button
      class="counter__button"
      on:click={() => ($initalCount >= 9 ? initalCount.set(0, { duration: 250 }) : $initalCount++)}
    >
      <span>+</span>
    </button>
  </div>

  <div class="counts">
    {#each Array(Math.floor($initalCount)) as _, i}
      <div class="counts__count">{i + 1}</div>
    {/each}
  </div>
</section>

<style lang="scss">
  section {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    gap: 2rem;
    max-width: 500px;
    width: 100%;
  }
  .counts {
    display: flex;
    gap: 1rem;
    font-size: 18px;
    min-height: 1.5rem;
  }

  .counter {
    display: flex;
    gap: 1.5rem;
    align-items: center;

    &__count {
      display: flex;
      justify-content: center;
      font-size: 30px;
      min-width: 50px;
    }

    &__button {
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 10px;
      font-size: 40px;
      width: 50px;
      height: 50px;
      background-color: wheat;
      appearance: none;
      border: none;
      outline: none;
      border-radius: 7px;
      position: relative;
    }

    &__button:hover {
      cursor: pointer;
    }

    &__button--dec span {
      margin-top: -3px;
    }

    &__button span {
      position: absolute;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
      display: block;
    }
  }
</style>
