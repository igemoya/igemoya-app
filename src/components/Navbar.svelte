<script lang="ts">
  import { Link } from "svelte-routing";
  export let selected;
</script>

<div id="navbarContainer">
  <div class="navbar30">
    {#if selected == 0}
      <img src="/images/home-filled.svg" id="iconSelected" alt="">
    {:else}
      <Link to="/app">
        <img src="/images/home.svg" alt="">
      </Link>
    {/if}
  </div>
  <div class="navbar40">
    {#if selected == 1}
      <img src="/images/camera-filled.svg" id="iconSelected" alt="">
    {:else}
    <Link to="/app/camera">
      <img src="/images/camera.svg" alt="">
    </Link>
    {/if}
  </div>
  <div class="navbar30">
    {#if selected == 2}
      <img src="/images/beard-filled.svg" id="iconSelected" alt="">
    {:else}
    <Link to="/app/me">
      <img src="/images/beard.svg" alt="">
    </Link>
    {/if}
  </div>
</div>

<style>
  #navbarContainer {
    background-color: #fff;
    position: absolute;
    bottom: env(safe-area-inset-bottom);
    left: 0;
    display: flex;
    flex-direction: row;
    border-top: 0.5px solid #e7e7e7;
    width: 100vw;
    height: 8vh;
  }

  img {
    max-width: 4vh;
    max-height: 6vw;
  }
  
  .navbar30 {
    width: 30vw;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .navbar40 {
    width: 40vw;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
</style>