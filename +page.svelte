<script>
  import { onMount } from "svelte";

  let name = '';
  let begruessungen = /** @type {string[]} */ ([]);
  let fehler = '';
  let showGreeting = false;

  // Begrüssungen aus localStorage laden
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
      fehler = "Eingabe darf nicht leer sein";
      showGreeting = false;
      return;
    }

    fehler = '';
    showGreeting = true;

    begruessungen = [...begruessungen, `Hello ${saubererName}`];
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

  // Einzelne Begrüssung löschen
  function loescheBegruessung(index) {
    begruessungen.splice(index, 1);
    begruessungen = [...begruessungen];
    speichern();
  }

  // Liste komplett leeren
  function listeLeeren() {
    begruessungen = [];
    speichern();
  }
</script>

<!-- HTML -->

<h1 class="text-3xl font-bold text-blue-700 mb-4">
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

{#if fehler}
  <p class="error">{fehler}</p>
{/if}

{#if showGreeting}
  <p>Hello {name}</p>
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
