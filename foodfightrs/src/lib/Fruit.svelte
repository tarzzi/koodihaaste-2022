<script>
  export let hp;
  export let atk;
  export let def;
  export let gre;
  export let speed = calcAttackSpeed(atk, def, gre);
  export let rng;
  export let color;

  function calcAttackSpeed(atk, def, gre) {
    let speed = (atk + def + gre);
    speed = parseFloat(speed.toFixed(2));
    console.log(speed);
    return speed;
  }
</script>

<div class="fruit">
  <img class="{color}" alt="wannabefruit" src="https://placekitten.com/200/{rng}" />
  <p class="fruit-stats" style="text-align:left;">
    <span class="fruit-stat fruit-stat-hp">HP {hp}</span><br />
    <span class="fruit-stat fruit-stat-atk">ATK {atk}</span><br />
    <span class="fruit-stat fruit-stat-def">DEF {def}</span><br />
    <span class="fruit-stat fruit-stat-spd">SPEED {speed}</span>
  </p>
</div>

<style>
  * {
    margin: 0;
    padding: 0;
  }
  img {
    border-radius: 50%;
    /*box-shadow: -15px 15px 1em rgb(90, 90, 90);*/
    width: 152px;
    right: -75px;
    top: -1px;
    position: absolute;
    will-change: filter;
    transition: filter 200ms;
  }
  .blue:hover {
    transition: filter 200ms;
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .red:hover {
    transition: filter 200ms;
    filter: drop-shadow(0 0 2em #ff3e00aa);
  }
  .fruit {
    border: 1px solid black;
    border-right: 0; 
    margin: 0 auto;
    width: 300px;
    position: relative;
    height: 150px;
  }
  .fruit-title {
    font-size: 18px;
    font-weight: 800;
  }
  .fruit-stats{
    margin-top: 15px;
    padding: 15px;
  }
  .fruit-stat {
    text-align: left;
    font-weight: 600;
    padding-left: 10px;
  }
</style>
