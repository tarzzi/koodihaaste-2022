<script>
  // Get selected fruits
  export let lFruit;
  export let rFruit;
  let time = 0;
  let timer;
  export let battleLog = [];

  // Get battle stats
  let lHp = lFruit[0];
  let lAtk = lFruit[1];
  let lDef = lFruit[2];
  let lSpeed = lFruit[3];
  let lImgSrc = lFruit[4];
  let lName = lFruit[5];

  let rHp = rFruit[0];
  let rAtk = rFruit[1];
  let rDef = rFruit[2];
  let rSpeed = rFruit[3];
  let rImgSrc = rFruit[4];
  let rName = rFruit[5];

  // Get battle log
  let battleLogEl = document.getElementById("log");
  let btnActionEl = document.getElementById("btn-action");

  //Attack counters

  let lAttackCount = 1;
  let rAttackCount = 1;

  function startBattle() {
    console.log("Staring battle with fruits: ", lFruit, rFruit);
    battleLog = [];
    let battleLogEl = document.getElementById("log");
    let btnActionEl = document.getElementById("btn-action");
    btnActionEl.disabled = true;

    // Start battle
    timer = setInterval(() => {
      time++;
      runBattle();
    }, 100);
  }

  // Run battle
  function runBattle() {
    // lSpeed and Rspeed define when attack happens
    // if timer is over multiple of speed, attack happens

    if (lHp <= 0 || rHp <= 0) {
      clearInterval(timer);
      time = 0;
      btnActionEl.disabled = false;
      return;
    }

    if (time >= lAttackCount * lSpeed) {
      let damage = lAtk - rDef;
      if (damage < 0) {
        damage = 0;
      }
      rHp -= damage;
      let logEntry = `${lName} attacks ${rName} for ${damage} damage`;
      battleLog.push(logEntry);
      lAttackCount++;
    }
    if (time >= rAttackCount * rSpeed) {
      let damage = rAtk - lDef;
      if (damage < 0) {
        damage = 0;
      }
      lHp -= damage;
      let logEntry = `${rName} attacks ${lName} for ${damage} damage`;
      battleLog.push(logEntry);
      rAttackCount++;
    }
  }
</script>

<div id="battle-log">
  <!-- Show battle log -->
  <button id="btn-action" on:click={() => startBattle()}>FIGHT</button>
  <ul id="log">
    {#each battleLog as log}
      <li>{log}</li>
    {/each}
  </ul>
</div>

<style>
  #battle-log {
    width: 100%;
    height: 100vh;
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
