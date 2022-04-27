<script lang="ts">
  import SlotButton from '$lib/SlotButton.svelte';

  let clicked = false;
</script>

<section>
  <SlotButton on:click={() => (clicked = !clicked)}>Click me</SlotButton>

  <h4 class:visible={clicked}>I don't use props.. I use "slots", pretty cool.</h4>
</section>

<style lang="scss">
  section {
    display: flex;
    flex-direction: column;
  }
  h4 {
    opacity: 0;
    transition: opacity 0.25s ease-out;

    &.visible {
      opacity: 1;
    }
  }
</style>
