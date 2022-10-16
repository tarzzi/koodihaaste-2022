<script>
  import Settings from "./Settings.svelte";

  // Get selected fruits
  export let lFruit;
  export let rFruit;
  export let gameTickSetting = 500;
  let time = 0;
  let timer;
  let battleLog = [];
  let fightStatus = "FIGHT!";

  let leftChamp;
  let rightChamp;

  let red = "";
  let blue = "";

  let settingsVisible = "hidden";

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

    // Same champion recognition
    if (rightChamp.name === leftChamp.name) {
      red = "🟥";
      blue = "🟦";
    } else {
      red = "";
      blue = "";
    }

    // Start battle
    timer = setInterval(() => {
      time++;
      fightStatus = `Fighting ... ${time}`;
      tickBattle();
      window.scrollTo({
        left: 0,
        top: document.body.scrollHeight,
        behavior: "smooth",
      });
    }, gameTickSetting);
  }

  // Run battle
  function tickBattle() {
    // battler.speed defines when attack happens
    // if timer is over attackspeed * attacks, attack happens
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
    } else if (time >= rightChamp.attackCounter * rightChamp.speed) {
      attack(rightChamp, leftChamp);
    } else {
      if (time % 10 === 0) {
      battleCast();
      }
    }
  }

  function battleCast(){
    let rng = Math.floor(Math.random() * 15) + 1;
        let cast;
        let casting = [
          "The crowd goes bananas!",
          "This is going to be quite a pickle...",
          "Someone threw a seed to the battlefield!",
          "And what is this?",
          "...",
          "What a fine day for veggie smash!",
          "What a crowd today, the stadium is packed to the limits",
          "The opponent seems quite confident",
          "*crickets chirping*",
          "The crowd is cheering for the challenger",
          "The challenger is looking quite confident",
          "Did you see that? I sure did!",
          "We are in for a treat today!",
          "There it goes!",
          "Right down the blender!",
          "We all know what's going to happen next"
        ];
        cast = casting[rng];
        battleLog.push({ text: `Tick ${time} :: ${cast}`, color: "lightslategray" });
        battleLog = battleLog;
  }
  function attack(attacker, defender) {
    let crit = calculateCrit();
    let critCasts = critCasting(crit);
    let critStatus = critCasts[0];
    let displayColor = critCasts[1];

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
    let crit;
    switch (critChance) {
      case 0:
        crit = 0;
        break;
      case 9:
        crit = 2;
        break;
      default:
        crit = 1;
    }
    return crit;
  }
  function critCasting(crit){
    let critStatus = "";
    let displayColor = "white";
    switch (crit) {
      case 0:
        crit = 0;
        critStatus = "💨 Missed!";
        displayColor = "yellow";
        break;
      case 2:
        crit = 2;
        critStatus = "💢 Critical hit!";
        displayColor = "red";
        break;
      default:
        break;
    }
    return [critStatus, displayColor];
  }
  // Reset game counters
  function resetBattleCounters() {
    let btnActionEl = document.getElementById("btn-action");
    btnActionEl.disabled = false;
    time = 0;
    fightStatus = "FIGHT!";
  }

  function openSettings() {
    settingsVisible = "visible";
    console.log(settingsVisible);
    settingsVisible = settingsVisible;
  }
</script>

<div class="center">
  <button on:click={() => openSettings()}>Settings</button>
  <Settings bind:settingsVisible bind:gameTickSetting />
  <button id="btn-action" on:click={() => startBattle()}>{fightStatus}</button>
</div>
<div id="battle-log">
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
