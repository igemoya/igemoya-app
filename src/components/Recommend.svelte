<script lang="ts">
  export let title:String;
  export let src:String;
  export let id:String;
</script>

<div style="background-image: url({src})" id="backgroundContainer">
  <div>
    <span class="w600">{title}</span>
  </div>
</div>

<style>
  #backgroundContainer {
    margin-right: 2vw;
    background-position: center;
    background-size: cover;
  }

  div {
    backdrop-filter: brightness(0.6) blur(3px);
    -webkit-backdrop-filter: brightness(0.6) blur(3px);
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 0.5em;
    height: 13vh;
    width: 40vw;
  }

  span {
    color: white;
    font-size: 1em;
  }
</style>