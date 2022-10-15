<script>
  import { loop_guard } from "svelte/internal";

  // Get selected fruits
  export let lFruit;
  export let rFruit;
  export let gameTickSetting = 20;
  let time = 0;
  let timer;
  let battleLog = [];
  let fightStatus = "BEGIN!";

  // Get battle stats
  let lHp;
  let lAtk;
  let lDef;
  let lSpeed;
  let lImgSrc;
  let lName;

  let rHp;
  let rAtk;
  let rDef;
  let rSpeed;
  let rImgSrc;
  let rName;

  // Get battle log
  let battleLogEl = document.getElementById("log");
  let btnActionEl = document.getElementById("btn-action");

  //Attack counters

  let lAttackCount = 1;
  let rAttackCount = 1;

  function startBattle() {
    lHp = lFruit[0];
    lAtk = lFruit[1];
    lDef = lFruit[2];
    lSpeed = lFruit[3];
    lImgSrc = lFruit[4];
    lName = lFruit[5];

    rHp = rFruit[0];
    rAtk = rFruit[1];
    rDef = rFruit[2];
    rSpeed = rFruit[3];
    rImgSrc = rFruit[4];
    rName = rFruit[5];

    battleLog = [];
    let btnActionEl = document.getElementById("btn-action");
    btnActionEl.disabled = true;
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
    // lSpeed and Rspeed define when attack happens
    // if timer is over multiple of speed, attack happens

    // Match Ends
    if (lHp <= 0 || rHp <= 0) {
      if (lHp <= 0) {
        battleLog.push({
          text: `Tick ${time} :: ${lName} has been defeated! 🏆Victory for ${rName}🏆!`,
          color: "green",
        });
      } else {
        battleLog.push({
          text: `Tick ${time} :: ${rName} has been defeated! 🏆Victory for ${lName}🏆!`,
          color: "green",
        });
      }

      battleLog = battleLog;
      resetBattleCounters();
      clearInterval(timer);
      return;
    }

    // Attack loops
    if (time >= lAttackCount * lSpeed) {
      let calculatedCrit = calculateCrit();
      let crit = calculatedCrit[0];
      let critStatus = calculatedCrit[1];
      let displayColor = calculatedCrit[2];

      let damage = lAtk * crit - rDef;
      if (damage < 0) {
        damage = 0;
      }
      rHp -= damage;
      battleLog.push({
        text: `Tick ${time} :: ${lName} attacks ${rName} for ${damage} damage ${critStatus}`,
        color: displayColor,
      });
      battleLog = battleLog;
      lAttackCount++;
    }
    if (time >= rAttackCount * rSpeed) {
      let calculatedCrit = calculateCrit();
      let rCrit = calculatedCrit[0];
      let rCritStatus = calculatedCrit[1];
      let displayColor = calculatedCrit[2];

      let damage = rAtk * rCrit - lDef;
      if (damage < 0) {
        damage = 0;
      }
      lHp -= damage;
      battleLog.push({
        text: `Tick ${time} :: ${rName} attacks ${lName} for ${damage} damage ${rCritStatus}`,
        color: displayColor,
      });
      battleLog = battleLog;
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
    fightStatus = "BEGIN!";
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
  #battle-log {
    width: 100%;
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
  .center{
    display: grid;
    grid-template-columns: auto;
  }
</style>
