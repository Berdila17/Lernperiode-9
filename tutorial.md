# Svelte Begrüssungs-App Tutorial

## Goal
In this tutorial, you will learn how to create a simple Svelte web application where users can enter their name, receive a greeting, and store multiple greetings in a list with additional features like validation, sorting, and local storage.

---

## Previous Knowledge
We'll assume you already know the basics of HTML, CSS and JavaScript. You should also understand simple concepts like variables, functions and conditions.

---

## What you'll learn
In this tutorial, you will learn:
- How to use Svelte reactive variables  
- How to handle user input with `bind:value`  
- How to use conditions (`{#if}`)  
- How to work with arrays and lists (`{#each}`)  
- How to prevent duplicate entries  
- How to use localStorage to save data  
- How to create dynamic UI updates  

---

## Tutorial

### 1. Create basic variables and state

```svelte
<script>
  let name = '';
  let begruessungen = [];
  let fehler = '';
</script>

2. Handle user input
<input bind:value={name} placeholder="Name:" />
3. Add a greeting function with validation
function begruessen() {
  const saubererName = name.trim();

  if (saubererName.length === 0) {
    fehler = "Eingabe darf nicht leer sein";
    return;
  }

  fehler = '';
  begruessungen = [...begruessungen, `Hello ${saubererName}`];
}
4. Display errors and greetings
{#if fehler}
  <p>{fehler}</p>
{/if}

<ul>
  {#each begruessungen as begruessung}
    <li>{begruessung}</li>
  {/each}
</ul>
5. Prevent duplicate names
const nameSchonVorhanden = begruessungen.some(e =>
  e.toLowerCase().includes(name.toLowerCase())
);
6. Save data using localStorage
localStorage.setItem("begruessungen", JSON.stringify(begruessungen));
7. Load data on start
import { onMount } from "svelte";

onMount(() => {
  const data = localStorage.getItem("begruessungen");
  if (data) {
    begruessungen = JSON.parse(data);
  }
});
8. Sort greetings alphabetically
function sortiere() {
  begruessungen = [...begruessungen].sort();
}
9. Show the last greeting
<p>Letzte Begrüssung: {begruessungen[begruessungen.length - 1]}</p>
Result


You now have a working Svelte application where users can:

Enter their name
Receive a greeting
Store greetings in a list
Prevent duplicate entries
Sort greetings
See the latest greeting
Keep data even after refreshing the page

## What could go wrong?
Forgetting to trim the input → empty names allowed
localStorage not updating → data not saved
Duplicate check not working correctly
Errors not resetting properly
Not using reactive updates → UI not refreshing
