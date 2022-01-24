<script lang="ts">
  import Buttons from "./components/Buttons.svelte";

  let card: Card;
  let cards: Card[] = [];
  let isLoading: boolean = false;
  let isError: boolean = false;

  const getCard = async (event: CustomEvent) => {
    isLoading = true;
    isError = false;
    if (card) cards = [card, ...cards];
    await fetch(
      `https://api.scryfall.com/cards/random?q=type%3Acreature+cmc%3D${event.detail}-border%3Asilver`
    )
      .then((response) => {
        if (!response.ok) {
          isLoading = false;
          isError = true;
          card = undefined;
          return;
        }
        response.json().then((data) => {
          isLoading = false;
          card = { image: data.image_uris.normal, name: data.name };
          isError = false;
        });
      })
      .catch((error) => {
        isLoading = false;
        isError = true;
        card = undefined;
      });
  };
</script>

<nav class="mana-value">
  <Buttons on:clickManaValue={getCard} />
</nav>
<main>
  <section class="current-card">
    <div class="current-card-wrapper">
      {#if card && !isLoading && !isError}
        <img src={card.image} alt={card.name} class="current-card-img" />
      {/if}
      {#if isError}
        <div class="message">
          <span class="no-wrap"> 指定したマナ総量のクリーチャーが</span>
          <span class="no-wrap"> 見つかりませんでした。</span>
        </div>
      {/if}
      {#if isLoading}
        <div class="message">読み込み中</div>
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

  img {
    border-radius: 4.2% / 3%;
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
    height: fit-content;
  }
  .current-card::before {
    content: "";
    display: block;
    padding-top: 139.344262%;
    background-color: #ddd;
    border-radius: 4.2% / 3%;
  }
  .current-card-wrapper {
    position: absolute;
    top: 0;
    width: 100%;
    height: 100%;
  }
  .current-card-img {
    width: 100%;
  }

  .message {
    padding: 1rem;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  .no-wrap {
    display: inline-block;
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
