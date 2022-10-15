<script>
  import Title from "./Title.svelte";
  import { each } from "svelte/internal";
  import Fruit from "./lib/Fruit.svelte";
  import Battle from "./lib/Battle.svelte";
  let selectedL;
  let selectedR;
  let rFruit;
  let lFruit;

  let mainBlue = "#1e90ff";
  let mainRed = "#ff1e1e";

  let indexL = 0;
  let indexR = 0;

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

  lFruit = fruits[0];
  rFruit = fruits[0];
  selectedR = fruits[0].name;

  function nextItem(isRight, isMoveRight) {
    if(!isRight){
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
    lFruit = fruits[indexL];
    selectedL = lFruit.name;
    
    }
    else{
      if (isMoveRight) {
        console.log("next right");
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
    console.log("index:",indexR," fruit name:",fruits[indexR]);
    rFruit = fruits[indexR];
    selectedR = rFruit.name;
    
    }

  }

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
      <button class="btn-left" on:click={()=>nextItem(0,0)}>←</button>
      <h2>{lFruit.name}</h2>
      <button type="button" class="btn-right" on:click={()=>nextItem(0,1)}>→</button>
      <Fruit
        color="red"
        hp={lFruit.stats.hp}
        atk={lFruit.stats.atk}
        def={lFruit.stats.def}
        speed={lFruit.stats.atk + lFruit.stats.def + lFruit.stats.gre}
        imgSrc={lFruit.imgUrl}
      />
    </div>
    <div class="card center" style="font-weight:bolder;font-size:28px;">VS</div>
    <div class="card blue">
      <button class="btn-left" on:click={()=>nextItem(1,0)}>←</button>
      <h2>{rFruit.name}</h2>
      <button type="button" class="btn-right" on:click={()=>nextItem(1,1)}>→</button>
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
</main>

<style>
  :root{
  --mainBlue: #1e90ff;
  --mainRed: #ff1e1e;
  }
  h2{
    text-transform: uppercase;
    display: inline;
  }
  .grid {
    display: grid;
    grid-template-columns: auto auto auto;
    border: solid 1px black;
  }
  button{
    padding: 5px 10px;
  font-family: monospace;
  border: none;
  background-color: transparent;
  color: white;
  font-size: 2em;
  }
  .red{
    background-color:  var(--mainRed);
    text-shadow: 0 0 10px black;
    color: white;
  }
  .blue{
    background-color:  var(--mainBlue);
    text-shadow: 0 0 10px black;
    color: white;
  }
  .center{
    color: white;
    text-shadow: 0 0 10px black;
    background: linear-gradient(
    to right,
    var(--mainRed) 0%,
    var(--mainRed) 50%,
    var(--mainBlue) 50%,
    var(--mainBlue) 100%
  );
  }
</style>
