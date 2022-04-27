<script lang="ts">
</script>

<button on:click>
  <slot />
</button>

<style lang="scss">
  button {
    display: inline-flex;
    width: max-content;
    margin: 0 auto;
    align-items: center;
    justify-content: center;
    padding: 10px;
    font-weight: 500;
    padding: 12px 30px;
    font-size: 18px;
    background-color: wheat;
    appearance: none;
    border: none;
    outline: none;
    border-radius: 7px;
    transition: opacity 0.15s linear;

    &:hover {
      opacity: 0.8;
      cursor: pointer;
    }
  }
</style>
