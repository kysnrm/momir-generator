<script lang="ts">
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  const clickManaValue = (manaValue: number) => {
    dispatch("clickManaValue", manaValue);
  };

  const maxManaValue = 16;
  const minManaValue = 0;
  const manaValues: number[] = Array.apply(
    null,
    new Array(maxManaValue + 1)
  ).map((v, i) => {
    return minManaValue + i;
  });
</script>

{#each manaValues as manaValue}
  <button class="mana-value-button" on:click={() => clickManaValue(manaValue)}
    >{manaValue}</button
  >
{/each}

<style>
  .mana-value-button {
    width: 3rem;
    height: 3rem;
    font-size: 1rem;
    font-weight: 700;
    border-radius: 0.5rem;
    background-color: transparent;
    border: 1px solid #aaa;
    cursor: pointer;
    outline: none;
    appearance: none;
  }
  .mana-value-button:hover,
  .mana-value-button:focus {
    background-color: #eee;
  }
  .mana-value-button + .mana-value-button {
    margin-left: 0.5rem;
  }
</style>
