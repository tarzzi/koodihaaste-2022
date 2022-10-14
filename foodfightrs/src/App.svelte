<script>
  import Fruit from "./lib/Fruit.svelte";
</script>

<main>
  <h1>FooD FighTRS</h1>

  <div class="grid">
    <div class="card">
      <select>
        <option>Test1</option>
        <option>Test2</option>
        <option>Test3</option>
        <option>Test4</option>
      </select>
      <h2>APPLE</h2>
      <Fruit
        color="blue"
        name="apple"
        hp="42"
        atk="5"
        def="6"
        gre="0.5"
        rng="201"
      />
    </div>
    <div class="card">VS.</div>
    <div class="card">
      <select>
        <option>Test1</option>
        <option>Test2</option>
        <option>Test3</option>
        <option>Test4</option>
      </select>
      <h2>PEAR</h2>
      <Fruit
        color="red"
        name="pear"
        hp="64"
        atk="2"
        def="7"
        gre="0.9"
        rng="202"
      />
    </div>
  </div>
</main>

<style>
  .grid {
    display: grid;
    grid-template-columns: auto auto auto;
  }
</style>
