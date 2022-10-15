<script>
  import Title from "./Title.svelte";

  import { each } from "svelte/internal";

  import Fruit from "./lib/Fruit.svelte";
  import Battle from "./lib/Battle.svelte";
  let selectedL;
  let selectedR;
  let rFruit;
  let lFruit;

  let fruits = [
    {
      id: 0,
      name: "Apple",
      stats: {
        hp: 42,
        atk: 12,
        def: 4,
        gre: 0.6,
      },
      imgUrl:
        "https://www.collinsdictionary.com/images/full/apple_158989157.jpg",
    },
    {
      id: 1,
      name: "Tomato",
      stats: {
        hp: 36,
        atk: 17,
        def: 5,
        gre: 0.6,
      },
      imgUrl:
        "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTgbpHnwBb8iuyggb-F5VFYa6FR4FgV7ksiveEhC8RVKA&s",
    },
    {
      id: 2,
      name: "Cucumber",
      stats: {
        hp: 29,
        atk: 10,
        def: 10,
        gre: 0.8,
      },
      imgUrl:
        "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcT-sWAabVWJFCcdZq7CuFb1OcdFYXRFxJSoyg&usqp=CAU",
    },
    {
      id: 3,
      name: "Banana",
      stats: {
        hp: 25,
        atk: 19,
        def: 7,
        gre: 0.2,
      },
      imgUrl:
        "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRPla48ZefOGRFbMQwoGg__vKIYxp_bV8XT02C1lRfWN8PyC7vKqJuDH1CN9lEqtW7QKJ8&usqp=CAU",
    },
    {
      id: 4,
      name: "Pear",
      stats: {
        hp: 46,
        atk: 12,
        def: 4,
        gre: 0.5,
      },
      imgUrl:
        "https://img.freepik.com/free-vector/vintage-pear-illustration_53876-112720.jpg?w=2000",
    },
  ];

  handleChange(0, "Apple");
  handleChange(1, "Apple");

  function handleChange(option, value) {
    let index = fruits.findIndex((object) => {
      return object.name === value;
    });

    let fruit = fruits[index].stats;
    let imgSrc = fruits[index].imgUrl;
    let speed = fruit.atk + fruit.def + fruit.gre;
    speed = parseFloat(speed.toFixed(2));

    if (option) {
      rFruit = [
        fruit.hp,
        fruit.atk,
        fruit.def,
        speed,
        imgSrc,
        fruits[index].name,
      ];
    } else {
      lFruit = [
        fruit.hp,
        fruit.atk,
        fruit.def,
        speed,
        imgSrc,
        fruits[index].name,
      ];
    }
  }
</script>

<main>
  <Title />
  <div class="grid">
    <div class="card red">
      <select
        bind:value={selectedL}
        on:change={() => handleChange(0, selectedL)}
      >
        {#each fruits as fruit}
          <option name={fruit.name}>{fruit.name}</option>
        {/each}
      </select>
      <h2>{selectedL}</h2>
      <Fruit
        color="red"
        hp={lFruit[0]}
        atk={lFruit[1]}
        def={lFruit[2]}
        speed={lFruit[3]}
        imgSrc={lFruit[4]}
      />
    </div>
    <div class="card center" style="font-weight:bolder;font-size:28px;">VS.</div>
    <div class="card blue">
      <select
        bind:value={selectedR}
        on:change={() => handleChange(1, selectedR)}
      >
        {#each fruits as fruit}
          <option name={fruit.name}>{fruit.name}</option>
        {/each}
      </select>
      <h2>{selectedR}</h2>
      <Fruit
        color="blue"
        hp={rFruit[0]}
        atk={rFruit[1]}
        def={rFruit[2]}
        speed={rFruit[3]}
        imgSrc={rFruit[4]}
      />
    </div>
  </div>
  <Battle {lFruit} {rFruit} />
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
  .red{
    background-color: red;
    color: white;
  }
  .blue{
    background-color: blue;
    color: white;
  }
  .center{
    color: white;
    background: linear-gradient(
    to right,
    red 0%,
    red 50%,
    blue 50%,
    blue 100%
  );
  }
</style>
