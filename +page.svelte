<script>
  // JS
  let name = '';
let begruessungen = /** @type {string[]} */ ([]);
  let fehler = '';
  let showGreeting = false;

  function begruessen() {
    const saubererName = name.trim();

    // Fehlerbehandlung
    if (saubererName.length === 0) {
      fehler = 'Eingabe darf nicht leer sein';
      showGreeting = false;
      return;
    }

    // Fehler zurücksetzen
    fehler = '';

    // Begrüssung anzeigen
    showGreeting = true;

    // Begrüssung speichern
    begruessungen = [...begruessungen, `Hello ${saubererName}`];

    // Timer: Nachricht nach 10 Sekunden ausblenden
    setTimeout(() => {
      showGreeting = false;
    }, 10000);
  }

  function loeschenName() {
    name = '';
    showGreeting = false;
    fehler = '';
  }
</script>

<!-- HTML -->
<h1>Tracer Bullet</h1>

<input bind:value={name} placeholder="Name:" />

<button on:click={begruessen}>
  Hallo sagen
</button>

<button on:click={loeschenName}>
  Clear
</button>

{#if fehler}
  <p class="error">{fehler}</p>
{/if}

{#if showGreeting}
  <p>Hello {name}</p>
{/if}

<h2>Gespeicherte Begrüssungen</h2>

<ul>
  {#each begruessungen as begruessung}
    <li>{begruessung}</li>
  {/each}
</ul>

<style>
  /* CSS */
  h1 {
    color: darkblue;
  }

  input {
    padding: 6px;
    margin-top: 10px;
  }

  p {
    font-weight: bold;
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
