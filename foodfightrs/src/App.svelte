<script>
  import { each } from "svelte/internal";

  import Fruit from "./lib/Fruit.svelte";

  let selectedL;
  let selectedR;
  let rhp;
  let ratk;
  let rdef;
  let rgre;
  let lhp;
  let latk;
  let ldef;
  export let lgre;

  let fruits = [
    {
      id: 0,
      name: "apple",
      stats: {
        hp: 42,
        atk: 5,
        def: 4,
        gre: 0.6,
      },
    },
    {
      id: 1,
      name: "tomato",
      stats: {
        hp: 36,
        atk: 7,
        def: 5,
        gre: 0.6,
      },
    },
    {
      id: 2,
      name: "cucumber",
      stats: {
        hp: 29,
        atk: 4,
        def: 10,
        gre: 0.8,
      },
    },
    {
      id: 3,
      name: "banana",
      stats: {
        hp: 25,
        atk: 2,
        def: 7,
        gre: 0.2,
      },
    },
    {
      id: 4,
      name: "pear",
      stats: {
        hp: 46,
        atk: 7,
        def: 1,
        gre: 0.5,
      },
    },
  ];

  handleChange(0 , "apple");
  handleChange(1 , "apple");


  function handleChange(option, value) {
    let index = fruits.findIndex((object) => {
      return object.name === value;
    });

    let fruit = fruits[index].stats;

    if (option) {
      rhp = fruit.hp;
      ratk = fruit.atk;
      rdef = fruit.def;
      rgre = fruit.gre;
    } else {
      lhp = fruit.hp;
      latk = fruit.atk;
      ldef = fruit.def;
      lgre = fruit.gre;
    }
  }
</script>

<main>
  <h1>FooD FighTRS</h1>

  <div class="grid">
    <div class="card">
      <select
        bind:value={selectedL}
        on:change={() => handleChange(0, selectedL)}
      >
        {#each fruits as fruit}
          <option name={fruit.name}>{fruit.name}</option>
        {/each}
      </select>
      <h2>{selectedL}</h2>
      <Fruit color="red" hp={lhp} atk={latk} def={ldef} gre={lgre} rng="202" />
    </div>
    <div class="card" style="font-weight:bolder;font-size:28px;">VS.</div>
    <div class="card">
      <select
        bind:value={selectedR}
        on:change={() => handleChange(1, selectedR)}
      >
        {#each fruits as fruit}
          <option name={fruit.name}>{fruit.name}</option>
        {/each}
      </select>
      <h2>{selectedR}</h2>
      <Fruit color="blue" hp={rhp} atk={ratk} def={rdef} gre={rgre} rng="202" />
    </div>
  </div>
</main>

<style>
  h2,
  select {
    text-transform: uppercase;
  }
  .grid {
    display: grid;
    grid-template-columns: auto auto auto;
  }
  select {
    padding: 5px 15px;
    padding-right: 25px;
    font-size: 20px;
  }
  option {
    padding: 5px 0;
  }
</style>
