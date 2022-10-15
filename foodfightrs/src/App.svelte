<script>
  import { each } from "svelte/internal";

  import Fruit from "./lib/Fruit.svelte";
  import Battle from "./lib/Battle.svelte"
  let selectedL;
  let selectedR;
  let rFruit;
  let lFruit;
  let battle;

  const startBattle =() => battle.start();

  let fruits = [
    {
      id: 0,
      name: "apple",
      stats: {
        hp: 42,
        atk: 12,
        def: 4,
        gre: 0.6,
      },
      imgUrl:"https://www.collinsdictionary.com/images/full/apple_158989157.jpg"
    },
    {
      id: 1,
      name: "tomato",
      stats: {
        hp: 36,
        atk: 17,
        def: 5,
        gre: 0.6,
      },
      imgUrl:"https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTgbpHnwBb8iuyggb-F5VFYa6FR4FgV7ksiveEhC8RVKA&s"
    },
    {
      id: 2,
      name: "cucumber",
      stats: {
        hp: 29,
        atk: 10,
        def: 10,
        gre: 0.8,
      },
      imgUrl:"https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT-sWAabVWJFCcdZq7CuFb1OcdFYXRFxJSoyg&usqp=CAU"
    },
    {
      id: 3,
      name: "banana",
      stats: {
        hp: 25,
        atk: 19,
        def: 7,
        gre: 0.2,
      },
      imgUrl:"https://tf-cmsv2-smithsonianmag-media.s3.amazonaws.com/filer/d5/24/d5243019-e0fc-4b3c-8cdb-48e22f38bff2/istock-183380744.jpg"
    },
    {
      id: 4,
      name: "pear",
      stats: {
        hp: 46,
        atk: 12,
        def: 4,
        gre: 0.5,
      },
      imgUrl:"https://img.freepik.com/free-vector/vintage-pear-illustration_53876-112720.jpg?w=2000"
    },
  ];

  handleChange(0 , "apple");
  handleChange(1 , "apple");


  function handleChange(option, value) {
    let index = fruits.findIndex((object) => {
      return object.name === value;
    });

    let fruit = fruits[index].stats;
    let imgSrc = fruits[index].imgUrl;
    let speed = (fruit.atk + fruit.def + fruit.gre);
    speed = parseFloat(speed.toFixed(2));
    console.log(speed);

    if (option) {
      rFruit = [fruit.hp, fruit.atk, fruit.def, speed, imgSrc, fruits[index].name];
    } else {
      lFruit = [fruit.hp, fruit.atk, fruit.def, speed, imgSrc, fruits[index].name];
    }
  }
</script>

<main>
  <h1>VFL</h1>
  <h3>Veggie Fighters League</h3>
  <h2>Select your champions</h2>
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
      <Fruit color="red" hp={lFruit[0]} atk={lFruit[1]} def={lFruit[2]} speed={lFruit[3]} imgSrc={lFruit[4]}/>
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
      <Fruit color="blue" hp={rFruit[0]} atk={rFruit[1]} def={rFruit[2]} speed={rFruit[3]} imgSrc={rFruit[4]}/>
    </div>
  </div>
  <hr>
  <Battle lFruit={lFruit} rFruit={rFruit} />
</main>

<style>
  h1{
    margin-bottom: 0;
    padding-bottom: 0;
  }
  h2,
  select {
    text-transform: uppercase;
  }
  h3{
    font-style: italic;
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
