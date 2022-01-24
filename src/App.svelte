<script lang="ts">
  import Buttons from "./components/Buttons.svelte";

  let card: Card;
  let cards: Card[] = [];
  let isError: boolean = false;

  const getCard = async (event: CustomEvent) => {
    if (card) cards = [card, ...cards];
    await fetch(
      `https://api.scryfall.com/cards/random?q=type%3Acreature+cmc%3D${event.detail}-border%3Asilver`
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

<nav class="mana-value">
  <Buttons on:clickManaValue={getCard} />
  {#if isError}
    指定したマナ総量のクリーチャーが見つかりませんでした。
  {/if}
</nav>
<main>
  <section class="current-card">
    <div class="current-card-wrapper">
      {#if card && !isError}
        <img src={card.image} alt={card.name} class="current-card-img" />
      {/if}
    </div>
  </section>
  <section class="history">
    {#each cards as card}
      <div class="history-card-wrapper">
        <img class="history-card" src={card.image} alt={card.name} />
      </div>
    {/each}
  </section>
</main>

<style>
  * {
    box-sizing: border-box;
  }

  .mana-value {
    margin-bottom: 1.5rem;
  }

  main {
    display: flex;
  }

  .current-card {
    margin-right: 1.5rem;
    position: relative;
    width: 33.333333%;
  }

  .current-card::before {
    content: "";
    display: block;
    padding-top: 139%;
    background-color: #aaa;
    border-radius: 0.25rem;
  }

  .current-card-wrapper {
    position: absolute;
    top: 0;
  }

  .current-card-img {
    width: 100%;
  }

  .history {
    display: flex;
    flex-wrap: wrap;
    align-content: flex-start;
    margin: -0.5rem;
    width: 66.666666%;
  }
  .history-card-wrapper {
    width: 33.333333%;
    padding: 0.5rem;
  }
  .history-card {
    width: 100%;
    vertical-align: top;
  }
</style>
