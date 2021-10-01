<script>
  let x = 0;
  let y = 0;

  $: {
    console.log("hola mundoo");
    console.log(`valor de x = ${x} `);
  }

  $: mivariable = x * 2;
  $: mivariable2 = 3;
</script>

<button on:click={() => x++}>
  Increment X {x}
</button>

<button on:click={() => y++}>
  Increment Y {y}
</button>

<p>mi variable va a cambiar porque es reactiva y depende de x y la puedo definir sin let directamente con el $: ={mivariable}</p>
<p>mi variable2 no cambiara aunque sea reactiva ya que no depende de nadie ={mivariable2}</p>

