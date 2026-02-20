<script>
  // JS
  let name = '';
</script>

<!-- HTML -->
<h1>Tracer Bullet</h1>

<input bind:value={name} placeholder="Laurentiu" />

<p>Hello {name}</p>

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
</style>
