<script>
  import { onMount } from "svelte";

  let name = '';
  let begruessungen = /** @type {string[]} */ ([]);
  let fehler = '';
  let showGreeting = false;
  let aktuelleBegruessung = '';

  const greetings = ['Hallo', 'Willkommen', 'Hi', 'Schön dich zu sehen'];

  onMount(() => {
    const gespeicherte = localStorage.getItem("begruessungen");
    if (gespeicherte) {
      begruessungen = JSON.parse(gespeicherte);
    }
  });

  function speichern() {
    localStorage.setItem("begruessungen", JSON.stringify(begruessungen));
  }

  function begruessen() {
    const saubererName = name.trim();

    if (saubererName.length === 0) {
      fehler = 'Eingabe darf nicht leer sein';
      showGreeting = false;
      return;
    }

    const nameSchonVorhanden = begruessungen.some((eintrag) =>
      eintrag.toLowerCase().includes(saubererName.toLowerCase())
    );

    if (nameSchonVorhanden) {
      fehler = 'Dieser Name wurde schon eingegeben';
      showGreeting = false;
      return;
    }

    fehler = '';

    const zufallsIndex = Math.floor(Math.random() * greetings.length);
    const greeting = greetings[zufallsIndex];

    aktuelleBegruessung = `${greeting} ${saubererName}`;
    showGreeting = true;

    begruessungen = [...begruessungen, aktuelleBegruessung];
    speichern();

    setTimeout(() => {
      showGreeting = false;
    }, 10000);
  }

  function loeschenName() {
    name = '';
    showGreeting = false;
    fehler = '';
  }

  function loescheBegruessung(index) {
    begruessungen.splice(index, 1);
    begruessungen = [...begruessungen];
    speichern();
  }

  function listeLeeren() {
    begruessungen = [];
    aktuelleBegruessung = '';
    speichern();
  }

  function sortiereBegruessungen() {
    begruessungen = [...begruessungen].sort((a, b) => a.localeCompare(b));
    speichern();
  }

  $: letzteBegruessung =
    begruessungen.length > 0 ? begruessungen[begruessungen.length - 1] : '';
</script>

<!-- HTML -->

<h1 class="text-4xl font-bold text-cyan-700 mb-4">
  Tracer Bullet
</h1>

<input bind:value={name} placeholder="Name:" />

<button on:click={begruessen}>
  Hallo sagen
</button>

<button on:click={loeschenName}>
  Clear
</button>

<button on:click={listeLeeren}>
  Liste leeren
</button>

<button on:click={sortiereBegruessungen}>
  A-Z sortieren
</button>

{#if fehler}
  <p class="error">{fehler}</p>
{/if}

{#if showGreeting}
  <p>{aktuelleBegruessung}</p>
{/if}

{#if letzteBegruessung}
  <p class="letzte">
    Letzte Begrüssung: {letzteBegruessung}
  </p>
{/if}

<h2>Gespeicherte Begrüssungen</h2>

<ul>
  {#each begruessungen as begruessung, index}
    <li>
      {begruessung}
      <button on:click={() => loescheBegruessung(index)}>
        Delete
      </button>
    </li>
  {/each}
</ul>

<style>
  input {
    padding: 6px;
    margin-top: 10px;
  }

  button {
    padding: 6px 10px;
    margin-left: 5px;
    background-color: rgb(0, 139, 132);
    color: white;
    border: none;
    cursor: pointer;
  }

  button:hover {
    opacity: 0.8;
  }

  .error {
    color: red;
    font-weight: bold;
    margin-top: 10px;
  }

  .letzte {
    color: darkblue;
    font-weight: bold;
    margin-top: 10px;
  }

  h2 {
    margin-top: 20px;
    color: rgb(0, 139, 132);
  }

  ul {
    margin-top: 10px;
    padding-left: 20px;
  }

  li {
    margin-bottom: 5px;
  }
</style>
