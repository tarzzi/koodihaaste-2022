<script>

  // Get selected fruits
  export let lFruit;
  export let rFruit;
  export let gameTickSetting = 20;
  let time = 0;
  let timer;
  let battleLog = [];
  let fightStatus = "FIGHT!";

  // Get battle stats
  let leftChamp;
  let rightChamp;

  let rigthAttackSpeed;
  let leftAttackSpeed;
  let lName;


  //Attack counters

  let lAttackCount = 1;
  let rAttackCount = 1;

  function startBattle() {
    leftChamp = lFruit;
    leftAttackSpeed = lFruit.stats.atk + lFruit.stats.def + lFruit.stats.gre;
    lName = lFruit.name;

    rightChamp = rFruit;
    rigthAttackSpeed = rFruit.stats.atk + rFruit.stats.def + rFruit.stats.gre;
    rightChamp.name = rFruit.name;

    battleLog = [];
    let btnActionEl = document.getElementById("btn-action");
    btnActionEl.disabled = true;
    console.log("L hp",leftChamp.stats.hp);
    battleLog.push({ text: `Tick 00 :: Battle started!` });
    console.log("Original stats", lFruit, rFruit);

    console.log("ChampionStats", leftChamp, rightChamp);
    // Start battle
    timer = setInterval(() => {
      time++;
      fightStatus = `Fighting ... ${time}`;
      tickBattle();
    }, gameTickSetting);
  }

  // Run battle
  function tickBattle() {
    // leftAttackSpeed and rigthAttackSpeed define when attack happens
    // if timer is over multiple of speed, attack happens

    let red = "";
    let blue = "";
    if (rightChamp.name === lName) {
      red = "🟥";
      blue = "🟦";
    }
    // Match Ends
    if (lFruit.stats.hp <= 0 || rightChamp.stats.hp <= 0) {
      if (lFruit.stats.hp <= 0) {
        battleLog.push({
          text: `Tick ${time} :: ${lName} has been defeated! 🏆Victory for ${blue}${rightChamp.name}🏆!`,
          color: "green",
        });
      } else {
        battleLog.push({
          text: `Tick ${time} :: ${rightChamp.name} has been defeated! 🏆Victory for ${red}${lName}🏆!`,
          color: "green",
        });
      }

      battleLog = battleLog;
      resetBattleCounters();
      clearInterval(timer);
      return;
    }

    // Attack loops
    if (time >= lAttackCount * leftAttackSpeed) {
      let calculatedCrit = calculateCrit();
      let crit = calculatedCrit[0];
      let critStatus = calculatedCrit[1];
      let displayColor = calculatedCrit[2];

      let damage = lFruit.stats.atk * crit - rightChamp.stats.def;
      if (damage < 0) {
        damage = 0;
      }
      rightChamp.stats.hp -= damage;
      let logItem = {
        text: `Tick ${time} :: ${red}${lName} attacks ${blue}${rightChamp.name} for ${damage} damage ${critStatus}`,
        color: displayColor,
      };
      battleLog = [...battleLog, logItem];
      lAttackCount++;
    }
    if (time >= rAttackCount * rigthAttackSpeed) {
      let calculatedCrit = calculateCrit();
      let rCrit = calculatedCrit[0];
      let rCritStatus = calculatedCrit[1];
      let displayColor = calculatedCrit[2];

      let damage = rightChamp.stats.atk * rCrit - lFruit.stats.def;
      if (damage < 0) {
        damage = 0;
      }
      lFruit.stats.hp -= damage;
      let logItem = {
        text: `Tick ${time} :: ${blue}${rightChamp.name} attacks ${red}${lName} for ${damage} damage ${rCritStatus}`,
        color: displayColor,
      };
      battleLog = [...battleLog, logItem];
      rAttackCount++;
    }
  }

  // Reset game counters
  function resetBattleCounters() {
    let btnActionEl = document.getElementById("btn-action");
    btnActionEl.disabled = false;
    lAttackCount = 1;
    rAttackCount = 1;
    time = 0;
    leftChamp = null;
    rightChamp = null;
    fightStatus = "FIGHT!";
  }

  function calculateCrit() {
    //Criticals and misses
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
</script>

<div class="center">
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
