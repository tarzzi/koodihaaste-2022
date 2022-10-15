<script>
  import { loop_guard } from "svelte/internal";

  // Get selected fruits
  export let lFruit;
  export let rFruit;
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

    console.log("Staring battle with fruits: ", lFruit, rFruit);
    battleLog = [];
    let battleLogEl = document.getElementById("log");
    let btnActionEl = document.getElementById("btn-action");
    btnActionEl.disabled = true;
    battleLog.push(`Tick 00 :: Battle started!`);

    // Start battle
    timer = setInterval(() => {
      time++;
      fightStatus = `Fighting ... ${time}`;
      tickBattle();
    }, 20);
  }

  // Run battle
  function tickBattle() {
    // lSpeed and Rspeed define when attack happens
    // if timer is over multiple of speed, attack happens

    // Match Ends
    if (lHp <= 0 || rHp <= 0) {
      if (lHp <= 0) {
        battleLog.push(
          `Tick ${time} :: ${lName} has been defeated! Victory for ${rName}!`
        );
      } else {
        battleLog.push(
          `Tick ${time} :: ${rName} has been defeated! Victory for ${lName}!`
        );
      }

      battleLog = battleLog;
      resetBattleCounters();
      clearInterval(timer);
      return;
    }

    // Attack loops
    if (time >= lAttackCount * lSpeed) {
      let calculatedCrit = calculateCrit();
      let lCrit = calculatedCrit[0];
      let lCritStatus = calculatedCrit[1];

      let damage = lAtk * lCrit - rDef;
      if (damage < 0) {
        damage = 0;
      }
      rHp -= damage;
      battleLog.push(
        `Tick ${time} :: ${lName} attacks ${rName} for ${damage} damage ${lCritStatus}`
      );
      battleLog = battleLog;
      lAttackCount++;
    }
    if (time >= rAttackCount * rSpeed) {
      //Criticals and misses
      let calculatedCrit = calculateCrit();
      let rCrit = calculatedCrit[0];
      let rCritStatus = calculatedCrit[1];

      let damage = rAtk * rCrit - lDef;
      if (damage < 0) {
        damage = 0;
      }
      lHp -= damage;
      battleLog.push(
        `Tick ${time} :: ${rName} attacks ${lName} for ${damage} damage ${rCritStatus}`
      );
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
    let critChance = Math.floor(Math.random() * 10);
    let critStatus = "";
    let crit;
    switch (critChance) {
      case 0:
        crit = 0;
        critStatus = "-- Missed!";
        break;
      case 9:
        crit = 2;
        critStatus = "-- Critical hit!";
        break;
      default:
        crit = 1;
    }
    return [crit, critStatus];
  }
</script>

<div id="battle-log">
  <!-- Show battle log -->
  <button id="btn-action" on:click={() => startBattle()}>{fightStatus}</button>
  <ul id="log">
    {#each battleLog as log}
      <li>{log}</li>
    {/each}
  </ul>
</div>

<style>
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
</style>
