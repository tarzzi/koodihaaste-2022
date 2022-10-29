<script>
  import Settings from "./Settings.svelte";

  // Get selected fruits
  export let lFruit;
  export let rFruit;
  export let gameTickSetting = 250;
  let time = 0;
  let timer;
  let battleLog = [];
  let fightStatus = "FIGHT!";
  let battleStarted = false;
  let leftChamp;
  let rightChamp;
  let leftHpLeft;
  let rightHpLeft;
  let red = "";
  let blue = "";

  let settingsVisible = "hidden";
  let logScrollType = "log_scroll";

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
    battleStarted = true;
    leftChamp = new Battler(lFruit);
    rightChamp = new Battler(rFruit);

    initializeHpMeter();
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
    if (logScrollType === "log_scroll") {
      continuousScroll();
    }

    // Start battle
    timer = setInterval(() => {
      time++;
      fightStatus = `Fighting ... ${time}`;
      tickBattle();
      scrollBattleLog();
    }, gameTickSetting);
  }

  let continuousScrollRunning = false;
  let scroller;

  function continuousScroll() {
    continuousScrollRunning = true;
    scroller = setInterval(() => {
      let out = document.getElementById("log");
      // allow 1px inaccuracy by adding 1
      const isScrolledToBottom =
        out.scrollHeight - out.clientHeight <= out.scrollTop + 1;
      console.log(isScrolledToBottom);
      // scroll to bottom if isScrolledToBottom is true
      if (!isScrolledToBottom) {
        out.scrollTop = out.scrollHeight - out.clientHeight;
      }
    }, 500);
  }
  function stopScroll() {
    continuousScrollRunning = false;
    clearInterval(scroller);
  }

  function scrollBattleLog() {
    if (logScrollType === "log_scroll" && !continuousScrollRunning) {
      setTimeout(() => {
        let out = document.getElementById("log");
        out.scrollTop = out.scrollHeight;
      }, 500);
    } else {
      window.scrollTo({
        left: 0,
        top: document.body.scrollHeight,
        behavior: "smooth",
      });
    }
  }
  function initializeHpMeter() {
    leftHpLeft = "❤️❤️❤️❤️❤️";
    rightHpLeft = "❤️❤️❤️❤️❤️";
  }
  function updateHpMeter(redHpNow, rightHpNow) {
    leftHpLeft = getRemainingHp(redHpNow);
    rightHpLeft = getRemainingHp(rightHpNow);
  }

  function getRemainingHp(hp) {
    let hpLeft = "";
    if (hp <= 0) {
      hpLeft = "🤍🤍🤍🤍🤍";
    } else if (hp <= 10) {
      hpLeft = "💔🤍🤍🤍🤍";
    } else if (hp <= 20) {
      hpLeft = "❤️🤍🤍🤍🤍";
    } else if (hp <= 30) {
      hpLeft = "❤️💔🤍🤍🤍";
    } else if (hp <= 40) {
      hpLeft = "❤️❤️🤍🤍🤍";
    } else if (hp <= 50) {
      hpLeft = "❤️❤️💔🤍🤍";
    } else if (hp <= 60) {
      hpLeft = "❤️❤️❤️🤍🤍";
    } else if (hp <= 70) {
      hpLeft = "❤️❤️❤️💔🤍";
    } else if (hp <= 80) {
      hpLeft = "❤️❤️❤️❤️🤍";
    } else if (hp <= 90) {
      hpLeft = "❤️❤️❤️❤️💔";
    } else {
      hpLeft = "❤️❤️❤️❤️❤️";
    }

    return hpLeft;
  }

  // Run battle
  function tickBattle() {
    updateHpMeter(leftChamp.hp, rightChamp.hp);
    // if timer is over attackspeed * attacks, attack happens
    // Match Ends
    if (leftChamp.hp <= 0 || rightChamp.hp <= 0) {
      battleLog.push({
        text: `Tick ${time} :: ################`,
        color: "green",
      });
      battleLog.push({
        text: `Tick ${time} :: GAME OVER`,
        color: "green",
      });
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
      battleLog.push({
        text: `Tick ${time} :: ################`,
        color: "green",
      });
      battleLog = battleLog;
      resetBattleCounters();
      clearInterval(timer);

      return;
    }

    // Attack loops
    if (time >= leftChamp.attackCounter * leftChamp.speed) {
      attack(leftChamp, rightChamp, "red");
    } else if (time >= rightChamp.attackCounter * rightChamp.speed) {
      attack(rightChamp, leftChamp, "blue");
    } else {
      if (time % 10 === 0) {
        battleCast();
      }
    }
  }

  function battleCast() {
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
      "We all know what's going to happen next",
    ];
    cast = casting[rng];
    battleLog.push({
      text: `Tick ${time} :: ${cast}`,
      color: "lightslategray",
    });
    battleLog = battleLog;
  }
  function attack(attacker, defender, color) {
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
      text: `Tick ${time <= 9 ? "0" + time : time} :: ${
        color == "red" ? red : blue
      } ${attacker.name} attacks ${color == "red" ? red : blue} ${
        defender.name
      } for ${damage} damage ${critStatus}`,
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
  function critCasting(crit) {
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
    stopScroll();
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
  <button class="btn_settings" on:click={() => openSettings()}>Settings</button>
  <Settings bind:settingsVisible bind:gameTickSetting bind:logScrollType />
  <button id="btn-action" on:click={() => startBattle()}>{fightStatus}</button>
</div>
<div id="battle-log">
  <h3>BATTLE LOG</h3>
  <ul class={logScrollType} id="log">
    {#each battleLog as log}
      <li style="color:{log.color}">{log.text}</li>
    {/each}
  </ul>
  <div>
    {#if !leftChamp}
      <div class="float-left">🟥</div>
      <div class="float-right">🟦</div>
    {:else}
      <div class="float-left">🟥 {leftHpLeft}</div>
      <div class="float-right">{rightHpLeft}🟦</div>
    {/if}
  </div>
  <div class="clearfix" />
</div>

<style>
  .clearfix {
    clear: both;
  }
  .float-left {
    float: left;
  }
  .float-right {
    float: right;
  }
  ul {
    list-style-type: "⏱️";
  }
  button {
    margin-bottom: 1em;
  }
  button {
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
    border: 5px #8c8c8c;
    border-style: ridge;
    margin: 0 auto;
  }
  #log {
    text-align: left;
  }
  .log_scroll {
    height: 95px;
    overflow: auto;
  }
  .log_full {
    height: 100%;
    overflow: auto;
  }
  .center {
    display: grid;
    grid-template-columns: auto;
  }
  .btn_settings {
    padding: 5px 0;
    font-size: 20px;
    border-bottom-left-radius: 8px;
    border-bottom-right-radius: 8px;
  }
  @media (max-width: 425px) {
    button {
      width: 90%;
      border-radius: 0.5em;
      margin-left: auto;
      margin-right: auto;
    }
    .btn_settings {
      margin-top: 1em;
      padding: 0.2em 1.2em;
    }
    #battle-log {
      font-size: 15px;
      width: auto;
    }
    #log {
      padding: 0;
    }
  }
  @media (prefers-color-scheme: dark) {
    button {
      color: rgb(226, 226, 226);
      background-color: #5f5f5f;
      border: solid 1px #c5c5c5;
    }
    button:active,
    button:hover {
      box-shadow: 2px 2px 2px rgb(156, 156, 156);
      color: rgb(224, 224, 224);
      border: solid 1px #c5c5c5;
    }
  }
</style>
