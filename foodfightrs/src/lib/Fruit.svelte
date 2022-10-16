<script>
  export let hp;
  export let atk;
  export let def;
  export let speed;
  export let color;
  export let imgSrc;
</script>

<div class="fruit">
  <img class={color} alt="wannabefruit" src={imgSrc} />
  {#if color === "red"}
    <div class="fruit-stats">
      <span class="">🧡</span>
      <span class="">{hp}</span>
      <span class="">🗡️</span>
      <span class="">{atk}</span>
      <span class="">🛡️ </span>
      <span class="">{def}</span>
      <span class="">⚡ </span>
      <span class="">{speed}</span>
    </div>
  {:else}
    <div class="fruit-stats right">
      <span class="">{hp}</span>
      <span class="">🧡</span>
      <span class="">{atk}</span>
      <span class="">🗡️</span>
      <span class="">{def}</span>
      <span class="">🛡️ </span>
      <span class="">{speed}</span>
      <span class="">⚡ </span>
    </div>
  {/if}
</div>

<style>
  * {
    margin: 0;
    padding: 0;
  }
  span {
    font-size: 22px;
    transition: all 500ms;
  }
  img {
    border-radius: 50%;
    box-shadow: 5px 5px 1em rgb(255, 255, 255), -5px -5px 1em rgb(255, 255, 255);
    width: 105px;
    height: 105px;
    left: 55%;
    top: 12px;
    position: absolute;
    will-change: filter;
    transition: filter 200ms;
    padding: 4px;
  }

  .blue:hover {
    transition: filter 200ms;
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .blue div img {
    left: 10%;
  }
  .red:hover {
    transition: filter 200ms;
    filter: drop-shadow(0 0 2em #ff3e00aa);
  }
  .fruit {
    margin: 0 auto;
    width: 300px;
    position: relative;
    height: 150px;
  }
  .fruit-title {
    font-size: 18px;
    font-weight: 800;
  }
  .fruit-stats {
    display: grid;
    float: left;
    grid-template-columns: auto auto;
    width: 80px;
    padding: 15px;
    margin-left: 1em;
    gap: 10px 0px;

  }
  .right{
    float: right;
    margin-right: 1em;
  }
</style>
