<script>
  import Title from "./lib/Title.svelte";
  import Fruit from "./lib/Fruit.svelte";
  import Battle from "./lib/Battle.svelte";
  import { onMount } from "svelte";
  let rFruit;
  let lFruit;

  let indexL = 0;
  let indexR = 0;

  function fruits2 (){
    fetch("https://raw.githubusercontent.com/tarzzi/koodihaaste-2022/main/fruits.json").then((response) => {
      return response.json();
    }).then((data) => {
      console.log(data);
    })};

  fruits2();

  let fruits = [
    {
      name: "Apple",
      stats: {
        hp: 42,
        atk: 12,
        def: 4,
        gre: 0.6,
      },
      imgUrl: "./images/apple.gif",
    },
    {
      name: "Pineapple",
      stats: {
        hp: 36,
        atk: 17,
        def: 5,
        gre: 0.6,
      },
      imgUrl: "./images/pineapple.gif",
    },
    {
      name: "Carrot",
      stats: {
        hp: 29,
        atk: 10,
        def: 10,
        gre: 0.8,
      },
      imgUrl: "./images/carrot.gif",
    },
    {
      name: "Banana",
      stats: {
        hp: 25,
        atk: 19,
        def: 7,
        gre: 0.2,
      },
      imgUrl: "./images/banana.gif",
    },
    {
      name: "Pear",
      stats: {
        hp: 46,
        atk: 12,
        def: 4,
        gre: 0.5,
      },
      imgUrl: "./images/pear.gif",
    },
    {
      name: "Blueberry",
      stats: {
        hp: 46,
        atk: 12,
        def: 4,
        gre: 0.5,
      },
      imgUrl: "./images/blueberry.gif",
    },
    {
      name: "Lemon",
      stats: {
        hp: 30,
        atk: 12,
        def: 4,
        gre: 0.5,
      },
      imgUrl: "./images/lemon.gif",
    },
    {
      name: "Pizza",
      stats: {
        hp: 221,
        atk: 10,
        def: 23,
        gre: 0.8,
      },
      imgUrl: "./images/pizza.gif",
    },
    {
      name: "Peach",
      stats: {
        hp: 46,
        atk: 12,
        def: 4,
        gre: 0.5,
      },
      imgUrl: "./images/peach.gif",
    },
    {
      name: "Pineapple",
      stats: {
        hp: 46,
        atk: 12,
        def: 4,
        gre: 0.5,
      },
      imgUrl: "./images/pineapple.gif",
    },
  ];

  lFruit = Object.assign({}, fruits[0]);
  rFruit = Object.assign({}, fruits[0]);

  function nextItem(isRightFruit, isMoveRight) {
    if (!isRightFruit) {
      if (isMoveRight) {
        indexL++;
        if (indexL > fruits.length - 1) {
          indexL = 0;
        }
      } else {
        indexL--;
        if (indexL < 0) {
          indexL = fruits.length - 1;
        }
      }
      lFruit = Object.assign({}, fruits[indexL]);
    } else {
      if (isMoveRight) {
        indexR++;
        if (indexR > fruits.length - 1) {
          indexR = 0;
        }
      } else {
        indexR--;
        if (indexR < 0) {
          indexR = fruits.length - 1;
        }
      }
      rFruit = Object.assign({}, fruits[indexR]);
    }
  }
  //preload images for better performance
  $: fruitUrls = fruits.map((fruit) => fruit.imgUrl);

</script>
<svelte:head>
    {#each fruitUrls as image}
      <link rel="preload" as="image" href={image} />
    {/each}
</svelte:head>
<main>
  <Title />
  <div class="grid">
    <div class="card red">
      <button class="btn-left red" on:click={() => nextItem(0, 0)}>←</button>
      <h2>{lFruit.name}</h2>
      <button
        type="button"
        class="btn-right red"
        on:click={() => nextItem(0, 1)}>→</button
      >
      <Fruit
        color="red"
        hp={lFruit.stats.hp}
        atk={lFruit.stats.atk}
        def={lFruit.stats.def}
        speed={lFruit.stats.atk + lFruit.stats.def + lFruit.stats.gre}
        imgSrc={lFruit.imgUrl}
      />
    </div>
    <div class="card blue">
      <button class="btn-left blue" on:click={() => nextItem(1, 0)}>←</button>
      <h2>{rFruit.name}</h2>
      <button
        type="button"
        class="btn-right blue"
        on:click={() => nextItem(1, 1)}>→</button
      >
      <Fruit
        color="blue"
        hp={rFruit.stats.hp}
        atk={rFruit.stats.atk}
        def={rFruit.stats.def}
        speed={rFruit.stats.atk + rFruit.stats.def + rFruit.stats.gre}
        imgSrc={rFruit.imgUrl}
      />
    </div>
  </div>
  <Battle {lFruit} {rFruit} />
  <div class="padding" />
  <div style="font-size:12px ;">
    Gifs by <a
      href="https://www.flaticon.com/animated-icons"
      title="pineapple animated icons">by Freepik - Flaticon</a
    >
  </div>
</main>

<style>
  :root {
    --mainBlue: #1e90ff;
    --mainRed: #ff1e1e;
  }
  h2 {
    text-transform: uppercase;
    display: inline;
  }
  .padding {
    height: 80px;
  }
  .card {
    position: relative;
  }
  .grid {
    display: grid;
    grid-template-columns: auto auto;
    border: solid 1px black;
  }
  .btn-left,
  .btn-right {
    cursor: pointer;
    padding: 2em 10px;
  }
  .btn-left:hover,
  .btn-right:hover {
    text-shadow: 0 0 10px white;
    background-color: #ffffff44;
  }
  .btn-left {
    height: 100%;
    position: absolute;
    top: 50%;
    left: 0;
    transform: translateY(-50%);
    font-size: 40px;
    font-weight: bolder;
  }
  .btn-right {
    height: 100%;
    position: absolute;
    top: 50%;
    right: 0;
    transform: translateY(-50%);
    font-size: 40px;
    font-weight: bolder;
  }
  button {
    padding: 5px 10px;
    font-family: monospace;
    border: none;
    background-color: transparent;
    color: white;
    font-size: 2em;
  }
  .red {
    background-color: var(--mainRed);
    text-shadow: 0 0 10px black;
    color: white;
  }
  .blue {
    background-color: var(--mainBlue);
    text-shadow: 0 0 10px black;
    color: white;
  }
  .card > button {
    display: none;
  }
  .card:hover button {
    display: inline-block;
    transition: background-color 200ms;
    transition: text-shadow 200ms;
  }

  @media (max-width: 768px) {
    .grid {
      border: none;
    }
  }

  @media (max-width: 425px) {
    .grid {
      display: inline;
    }
    .card > button {
      display: inline-block;
    }
  }
</style>
