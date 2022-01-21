<script lang="ts">
  const maxManaValue = 16;
  const minManaValue = 0;
  const manaValues: number[] = Array.apply(
    null,
    new Array(maxManaValue + 1)
  ).map((v, i) => {
    return minManaValue + i;
  });

  let card;
  let isError: boolean = false;

  const getCard = async (manaValue: number) => {
    await fetch(
      `https://api.scryfall.com/cards/random?q=type%3Acreature+cmc%3D${manaValue}-border%3Asilver`
    )
      .then((response) => {
        if (!response.ok) {
          isError = true;
          return;
        }
        response.json().then((data) => {
          card = data;
          isError = false;
        });
      })
      .catch((error) => {
        isError = true;
      });
  };
</script>

<main>
  <div>
    {#each manaValues as manaValue}
      <button on:click={() => getCard(manaValue)}>{manaValue}</button>
    {/each}
  </div>
  {#if card && !isError}
    <img src={card.image_uris.normal} alt={card.name} />
  {/if}
  {#if isError}
    指定したマナ総量のクリーチャーが見つかりませんでした。
  {/if}
</main>

<style>
</style>
