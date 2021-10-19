<script>
  import { count, gretting } from "./stores";
  import Increment from "./Increment.svelte";
  import Reset from "./Reset.svelte";
  let saludo = "Hola mundo";

  // esto es lo que nos ahorramos lo haces sin el $count .. count es un objeto writable que tiene como propiedades subscribe,set,update. cualquier objeto que tenga una propiedad de tipo subscribe svelte lo considera un store y trabajara como store es decir puedes utilizar $count = a cualquier cosa actualizarlo o simplemente leerlo.
  /*   const handleDecrement = () => {
      count.update(n => n -1)
  } */

  // reptio cualquier objeto que tenga la propiedad subscribe es tratado como un store y por lo tanto puedes modificar o leer el valor simplemnete con $count esto hara el trabajo de suscribirse
  // el $ lo hace leible y modificable ese es el truco.
  const handleDecrement = () => {
    $count = $count - 1;
  };

  // esto es lo que nos  ahorramos al utilizar $ aqui lo unico que se hace es recoger el valor cada vez que cambia.
  // utilizar count.set count.update count.subscribe nos ahorramos con el $

  /*   let count_global;

  const unsuscribe = count.subscribe((value) => {
      count_global = value
  })
  
*/
</script>

<h1>{saludo}</h1>
<p>{$count}</p>
<Increment />
<Reset />
<button on:click={handleDecrement}>DECREMENT</button>
<p>{$gretting}</p>
<!-- como ves podemos hacer bind del store tambien -->
<input placeholder="escribe algo" type="text" bind:value={$gretting} />
