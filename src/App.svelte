<script lang="ts">
  const maxManaValue = 16;
  const minManaValue = 0;
  const manaValues: number[] = Array.apply(
    null,
    new Array(maxManaValue + 1)
  ).map((v, i) => {
    return minManaValue + i;
  });

  let card: number = 0;

  const getCard = function (manaValue: number) {
    card = manaValue;
    console.log(card);
  };
</script>

<main>
  <div>
    {#each manaValues as manaValue}
      <button on:click={() => getCard(manaValue)}>{manaValue}</button>
    {/each}
  </div>
  {card}
</main>

<style>
</style>
