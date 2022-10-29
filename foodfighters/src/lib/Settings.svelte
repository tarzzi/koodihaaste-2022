<script>
  export let gameTickSetting;
  export let settingsVisible;
  export let logScrollType;
  export let scrollEnabled;

  let mediaQueryObj = window.matchMedia("(prefers-color-scheme: dark)");
  let isDarkMode = mediaQueryObj.matches;
  const isReduced = window.matchMedia("(prefers-reduced-motion: reduce)").matches === true;

  if (isReduced) {
    scrollEnabled = "false";
    console.log("Reduced motion detected");
  } else {
    scrollEnabled = "true";
    console.log("Reduced motion not detected");
  }
  console.log(isReduced);
  function closeSettings() {
    settingsVisible = "hidden";
  }
  const explanations = [
    "🧡 = Hitpoints - the amount of damage a food can take (Energy Kcal)",
    "🗡️ = Attack - attack points (Carbohydrates)",
    "🛡️ = Defence - reduces damage taken (Proteins)",
    "⚡ = Speed - how many ticks needed for attack (Carbohydrates + Proteins + Fat)",
  ];
</script>

<div id="settings" class={settingsVisible}>
  <div id="gamespeed">
    <div>
      <h3>Food Stats</h3>
      <ul>
        {#each explanations as expl}
          <li>{expl}</li>
        {/each}
      </ul>
    </div>
    <div class="settings_grid">
      <div class="settings_grid_item">
        <label>
          <h3>Game speed</h3>
          <input
            type="range"
            bind:value={gameTickSetting}
            min="10"
            step="10"
            max="1000"
          />
        </label>
        <p class="nomargintop">
          {gameTickSetting}ms
        </p>
      </div>
      <div class="settings_grid_item">
        <label>
          <h3>Battle log type</h3>
          <select bind:value={logScrollType}>
            <option value="log_scroll" selected>Scroll log</option>
            <option value="log_full">Full log</option>
          </select>
        </label>
      </div>
      <div class="settings_grid_item">
        <h3>Darkmode</h3>
        <span>
          Darkmode is currently <b>{isDarkMode ? "ON" : "OFF"}</b><br />
          <small>Change your
            browser prefers-color-scheme to change this</small>
        </span>
      </div>
      <div class="settings_grid_item">
        <label>
          <h3>Automatic scrolling</h3>
          <select bind:value={scrollEnabled}>
              <option value="true" >Enabled</option>
              <option value="false">Disabled</option>
          </select>
        </label>
      </div>
    </div>
    <button
      class="small"
      on:click={() => {
        closeSettings();
      }}>Close</button
    >
  </div>
</div>

<style>
  h3 {
    margin: 0;
    margin-bottom: 10px;
  }
  .nomargintop {
    margin-top: 0;
  }
  .settings_grid {
    display: grid;
    grid-template-columns: auto auto;
  }
  ul {
    list-style-type: none;
    text-align: left;
  }
  button {
    margin-bottom: 1em;
  }
  select {
    width: 50%;
      height: 25px;
      padding: 2px;
  }
  .small {
    font-size: 0.8em;
    margin-top: 1em;
    transition: translate 200ms;
  }
  .small:hover {
    translate: -1px -1px;
    transition: translate 200ms;
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
  input[type="range"]::-webkit-slider-runnable-track {
    width: 100%;
    cursor: pointer;
    box-shadow: 1px 1px 1px #000000, 0px 0px 1px #0d0d0d;
    background: #e9e9e9;
    border-radius: 1.3px;
    border: 0.2px solid #010101;
  }

  #settings {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    z-index: 1;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .hidden {
    display: none !important;
  }
  .visible {
    display: flex;
  }
  #gamespeed {
    background-color: #fff;
    width: auto;
    margin: 0 auto;
    margin-bottom: 1em;
    padding: 20px 30px;
  }
  input[type="range"] {
    margin-top: 5px;
  }

  @media (max-width: 425px) {
    .settings_grid {
      display: grid;
      grid-template-columns: auto;
      gap: 15px;
    }
    .settings_grid_item {
      margin-bottom: 10px;
    }
    select {
      height: 25px;
      padding: 2px;
    }
    h3 {
      margin: 0;
      margin-bottom: 5px;
    }
  }

  @media (prefers-color-scheme: dark) {
    button {
      color: #e9e9e9;
    }
    button:active {
      color: #e9e9e9;
    }
    #gamespeed {
      background-color: rgb(32, 32, 32);
      border: 1px solid white;
    }
    input[type="range"]::-webkit-slider-runnable-track {
      width: 100%;
      cursor: pointer;
      box-shadow: 1px 1px 1px #000000, 0px 0px 1px #0d0d0d;
      background: #a0a0a0;
      border-radius: 1.3px;
      border: 0.2px solid #010101;
    }
  }
</style>
