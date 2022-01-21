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

  let buttons: HTMLElement[] = [];
  let lastPressed: number;

  const keyDown = (event: KeyboardEvent) => {
    const key = event.key;
    if (!key.match(/[0-9]/)) return;
    const numericKey = Number(key);
    if (lastPressed === 1 && numericKey <= 6) {
      const target = 10 + numericKey;
      buttons[target].focus();
      lastPressed = numericKey;
      return;
    }
    lastPressed = numericKey;
    buttons[numericKey].focus();
  };
</script>

<svelte:window on:keydown={keyDown} />

{#each manaValues as manaValue, index}
  <button
    class="mana-value-button"
    on:click={() => clickManaValue(manaValue)}
    bind:this={buttons[index]}>{manaValue}</button
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
