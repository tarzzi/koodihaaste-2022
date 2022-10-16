<script>
  import Title from "./lib/Title.svelte";
  import Fruit from "./lib/Fruit.svelte";
  import Battle from "./lib/Battle.svelte";
  let rFruit;
  let lFruit;

  let indexL = 0;
  let indexR = 0;

  let fruits = [
    {
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


  lFruit = Object.assign({}, fruits[0]);
  rFruit = Object.assign({}, fruits[0]);

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
    lFruit = Object.assign({},fruits[indexL]);
    
    }
    else{
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


</script>

<main>
  <Title />
  <div class="grid">
    <div class="card red">
      <button class="btn-left red" on:click={()=>nextItem(0,0)}>←</button>
      <h2>{lFruit.name}</h2>
      <button type="button" class="btn-right red" on:click={()=>nextItem(0,1)}>→</button>
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
      <button class="btn-left blue" on:click={()=>nextItem(1,0)}>←</button>
      <h2>{rFruit.name}</h2>
      <button type="button" class="btn-right blue" on:click={()=>nextItem(1,1)}>→</button>
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
  <Battle lFruit={lFruit} rFruit={rFruit} />
  <div class="padding"></div>
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
  .padding{
    height: 80px;
  }
  .card{
    position: relative;
  }
  .grid {
    display: grid;
    grid-template-columns: auto auto;
    border: solid 1px black;
  }
  .btn-left,  .btn-right{
    cursor: pointer;
    padding: 2em 10px;
  }
  .btn-left:hover,  .btn-right:hover{
    text-shadow: 0 0 10px white;
    background-color: #ffffff44;
  }
  .btn-left{
    height: 100%;
    position: absolute;
    top: 50%;
    left: 0;
    transform: translateY(-50%);
    font-size: 40px;
    font-weight: bolder;
  }
  .btn-right{
    height: 100%;
    position: absolute;
    top: 50%;
    right: 0;
    transform: translateY(-50%);
    font-size: 40px;
    font-weight: bolder;
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
</style>
