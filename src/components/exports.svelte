<script>
  export let nombre;
  export let apellido = "Suarez";
  const edad = 28;

  console.log(`soy el componente exportacion`)
</script>

<div class="body">
  
  <h3>soy el componente nested o export fijate en los props y el nombre de exportacion</h3>
  <h4>Fijate que el nombre de archivo esta en minuscula lo unico que te pide svelte es que al importar el componente lo capitalices</h4>
  <p>
    Hola {nombre}
    {apellido} con edad {edad}
  </p>
  <p>
    y los props passados a este componente son {JSON.stringify($$props)}
  </p>
</div>

<style>

  .body {
    background:#32182b;
    color: gray;
  }
  
</style>


