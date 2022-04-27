<script>
  import '$lib/global.css';
</script>

<nav>
  <a href="/timer">Timer</a>
  <a href="/counter">Counter</a>
  <a href="/data">Data</a>
</nav>

<section class="center">
  <slot />
</section>

<style>
  nav {
    display: flex;
    gap: 1.5rem;
    align-items: center;
    justify-content: center;
  }

  .center {
    height: 100%;
    width: 100%;
    display: grid;
    place-items: center;
    margin-top: -3rem;
  }

  a {
    color: white;
    text-transform: uppercase;
    text-decoration: none;
    font-size: 1.5rem;
    opacity: 0.8;
    position: relative;

    transition: opacity 0.25s ease-out;
  }

  a:hover {
    opacity: 1;
  }

  a:after {
    content: '';
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    bottom: 0;
    height: 1px;
    background-color: wheat;
    width: 0%;

    transition: width 0.15s ease-out;
  }

  a:hover::after {
    width: 100%;
  }
</style>
