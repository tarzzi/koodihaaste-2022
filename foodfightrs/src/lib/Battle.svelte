<script>
  // Get selected fruits
  export let lFruit;
  export let rFruit;
  export let gameTickSetting = 20;
  let time = 0;
  let timer;
  let battleLog = [];
  let fightStatus = "FIGHT!";

  let leftChamp;
  let rightChamp;

  let red = "";
  let blue = "";

  // Battler object
  class Battler {
    constructor(fruit) {
      this.name = fruit.name;
      this.hp = fruit.stats.hp;
      this.atk = fruit.stats.atk;
      this.def = fruit.stats.def;
      this.speed = fruit.stats.atk + fruit.stats.def + fruit.stats.gre;
      this.imgSrc = fruit.imgUrl;
      this.attackCounter = 1;
    }
  }


  function startBattle() {
    // Assign champions
    leftChamp = new Battler(lFruit);
    rightChamp = new Battler(rFruit);

    // Disable start button
    let btnActionEl = document.getElementById("btn-action");
    btnActionEl.disabled = true;

    // Start logging
    battleLog = [];
    battleLog.push({ text: `Tick 00 :: Battle started!` });

    // Start battle
    timer = setInterval(() => {
      time++;
      fightStatus = `Fighting ... ${time}`;
      tickBattle();
    }, gameTickSetting);
  }

  // Run battle
  function tickBattle() {
    // battler.speed defines when attack happens
    // if timer is over attackspeed * attacks, attack happens
    // Same champion recognition
    if (rightChamp.name === leftChamp.name) {
      red = "🟥";
      blue = "🟦";
    }

    // Match Ends
    if (leftChamp.hp <= 0 || rightChamp.hp <= 0) {
      if (leftChamp.hp <= 0) {
        battleLog.push({
          text: `Tick ${time} :: ${leftChamp.name} has been defeated! 🏆Victory for ${blue}${rightChamp.name}🏆!`,
          color: "green",
        });
      } else {
        battleLog.push({
          text: `Tick ${time} :: ${rightChamp.name} has been defeated! 🏆Victory for ${red}${leftChamp.name}🏆!`,
          color: "green",
        });
      }
      battleLog = battleLog;
      resetBattleCounters();
      clearInterval(timer);
      return;
    }

    // Attack loops
    if (time >= leftChamp.attackCounter * leftChamp.speed) {
      attack(leftChamp, rightChamp);
    }

    if (time >= rightChamp.attackCounter * rightChamp.speed) {
      attack(rightChamp, leftChamp);
    }
  }

  // Create a reusable attack function from if statement above
  function attack(attacker, defender) {
    let calculatedCrit = calculateCrit();
    let crit = calculatedCrit[0];
    let critStatus = calculatedCrit[1];
    let displayColor = calculatedCrit[2];

    let damage = attacker.atk * crit - defender.def;
    if (damage < 0) {
      damage = 0;
    }
    defender.hp -= damage;
    let logItem = {
      text: `Tick ${time} :: ${blue}${attacker.name} attacks ${red}${defender.name} for ${damage} damage ${critStatus}`,
      color: displayColor,
    };
    battleLog = [...battleLog, logItem];
    attacker.attackCounter = attacker.attackCounter + 1;
  }

  //Criticals and misses
  function calculateCrit() {
    let critChance = Math.floor(Math.random() * 10);
    let critStatus = "";
    let displayColor = "white";
    let crit;
    switch (critChance) {
      case 0:
        crit = 0;
        critStatus = "💨 Missed!";
        displayColor = "yellow";
        break;
      case 9:
        crit = 2;
        critStatus = "💢 Critical hit!";
        displayColor = "red";
        break;
      default:
        crit = 1;
    }
    return [crit, critStatus, displayColor];
  }

  // Reset game counters
  function resetBattleCounters() {
    let btnActionEl = document.getElementById("btn-action");
    btnActionEl.disabled = false;
    time = 0;
    fightStatus = "FIGHT!";
  }
</script>

<div class="center">
  <div id="gamespeed">
    <label>
      Game speed: {gameTickSetting}ms
      <input type="range" bind:value={gameTickSetting} min="100" max="1000" />
    </label>
  </div>
  <button id="btn-action" on:click={() => startBattle()}>{fightStatus}</button>
</div>
<div id="battle-log">
  <!-- Show battle log -->
  <h3>BATTLE LOG</h3>
  <ul id="log">
    {#each battleLog as log}
      <li style="color:{log.color}">{log.text}</li>
    {/each}
  </ul>
</div>

<style>
  ul {
    list-style-type: "⏱️";
  }
  button {
    margin-bottom: 1em;
  }

  button {
    border-radius: 8px;
    color: rgb(78, 78, 78);
    border: 1px solid gray;
    padding: 0.6em 1.2em;
    font-size: 1.5em;
    font-weight: 800;
    font-family: monospace;
    cursor: pointer;
    transition: border-color 0.25s;
    transition: box-shadow 200ms;
  }
  button:hover {
    border-color: #646cff;
    box-shadow: 5px 5px 5px rgb(156, 156, 156);
    color: black;
  }
  button:active {
    box-shadow: 2px 2px 2px rgb(156, 156, 156);
    color: rgb(43, 43, 43);
  }
  /* 
button:focus,
button:focus-visible {
  outline: 4px auto -webkit-focus-ring-color;
} */
  #gamespeed {
    background-color: #fff;
    width: auto;
    margin: 0 auto;
    margin-bottom: 1em;
    padding: 20px 30px;
  }
  #battle-log {
    height: auto;
    background-color: #000;
    color: #fff;
    font-size: 20px;
    padding: 10px;
  }
  #log {
    width: 100%;
    height: 100%;
    overflow: auto;
    text-align: left;
  }
  .center {
    display: grid;
    grid-template-columns: auto;
  }
</style>
