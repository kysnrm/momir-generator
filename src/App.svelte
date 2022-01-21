<script lang="ts">
  const maxManaValue = 16;
  const minManaValue = 0;
  const manaValues: number[] = Array.apply(
    null,
    new Array(maxManaValue + 1)
  ).map((v, i) => {
    return minManaValue + i;
  });

  let card: Card;
  let cards: Card[] = [];
  let isError: boolean = false;

  const getCard = async (manaValue: number) => {
    if (card) cards = [card, ...cards];
    await fetch(
      `https://api.scryfall.com/cards/random?q=type%3Acreature+cmc%3D${manaValue}-border%3Asilver`
    )
      .then((response) => {
        if (!response.ok) {
          isError = true;
          card = undefined;
          return;
        }
        response.json().then((data) => {
          card = { image: data.image_uris.normal, name: data.name };
          isError = false;
        });
      })
      .catch((error) => {
        isError = true;
        card = undefined;
      });
  };
</script>

<main>
  <div>
    {#each manaValues as manaValue}
      <button on:click={() => getCard(manaValue)}>{manaValue}</button>
    {/each}
    {#if isError}
      指定したマナ総量のクリーチャーが見つかりませんでした。
    {/if}
  </div>
  {#if card && !isError}
    <img src={card.image} alt={card.name} />
  {/if}
  {#each cards as card}
    <img src={card.image} alt={card.name} />
  {/each}
</main>

<style>
</style>
