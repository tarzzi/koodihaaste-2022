<script>
  import Title from "./lib/Title.svelte";
  import Fruit from "./lib/Fruit.svelte";
  import Battle from "./lib/Battle.svelte";
  let rFruit;
  let lFruit;
  let indexL = 0;
  let indexR = 0;
  let endpoint =
    "https://raw.githubusercontent.com/tarzzi/koodihaaste-2022/main/fruits.json";
  let fruits = [];

  async function fetchFruits() {
    const response = await fetch(endpoint);
    const data = await response.json();
    data.fruits.map((fruit) => {
      let singleFruit = {
        name: fruit.name,
        imgUrl: fruit.imageUrl,
        stats: {
          hp: fruit.stats.energyKcal,
          atk: fruit.stats.carbohydrate,
          def: fruit.stats.protein,
          gre: fruit.stats.fat,
        },
      };
      fruits.push(singleFruit);
    });
    console.log(fruits);
    lFruit = Object.assign({}, fruits[0]);
    rFruit = Object.assign({}, fruits[0]);
  }

  let promise = fetchFruits();

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
      
      {#await promise}
        <p>Loading Champions...</p>
      {:then data }
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
        imgSrc={lFruit.imgUrl}/>
      {/await}
    </div>
    <div class="card blue">
      <button class="btn-left blue" on:click={() => nextItem(1, 0)}>←</button>

      {#await promise}
        <p>Loading Champions...</p>
      {:then data} 
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
      {/await}

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
