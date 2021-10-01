<script>
  let persona = {
    nombre: {
      value: undefined,
      regex: /^[a-zA-Z]+(\s{1}[a-zA-Z]+)*$/,
      error: true,
      errorInfo:
        "Debe comenzar y terminar con letras no se aceptan numeros ni caracteres especiales ni dobles espacios entre palabras",
    },
    email: { value: undefined, regex: /^[a-zA-Z0-9@.]{5,20}$/, error: true },
    edad: { value: undefined, error: true },
    sexo: { value: "", error: true }, // on:change de radio input al obtener el binding en la funcion handle me daba undefined pero event.target.value si tenia valor entonces por eso opte utilizaar los beneficios de svelte haciendo todo reactivo.
    pais: { value: "", error: true }, // ojo este vaue le paso al select inicialmente como valor del select pero si fuera undefined coje el primer valor de la lista de opciones. En este caso como el valor es "" y ninguna opcion tiene el value="" entonces no seleccionanada y queda en blanco.
    redes_sociales: { value: "", error: true }, // ojo un bind:gruoup jamas puede ser undefined, el solito arma el arreglo cuando le doy en los checks
    idiomas: { value: [], error: true },
  };

  $: isFormValid = validateForm(persona);
  let isAgree = false;
  let name;

  const paises = ["Estados Unidos", "Ecuador", "Peru", "Chile", "Argentina"];
  const redes_sociales = ["telegram", "instagram", "facebook", "whatsapp"];
  const idiomas = ["English", "Español"];

  // Usando la reactividad no tengo que usar handlekeyup para manejar directamente como en angular era.

  $: if (persona && name) {
    let { value, regex } = persona[name];
    console.log(value, name, value?.length);
    if (!value ) {
      console.log("entrando porque es vacio");
      persona[name].error = true;
    } else if (value && !regex) {
      console.log("entrando porque no es vacio y no tiene regex", name, value);
      persona[name].error = false;
    } else if (value && regex) {
      persona[name].error = !regex.test(value);
    }
  }
  

  const validateForm = (object) => {
    for (const key in persona) {
      const { error } = persona[key];
      // console.log(error,key,persona[key])
      if (error) {
        console.log("puto retornando falso");
        return false;
      }
    }
    console.log("puto retornando true");
    return true;
  };

  const handleChange = (e) => {
    console.log("handlechange", e.target.value,persona.idiomas.value);
    name = e.target.name;
  };
</script>

<div class="body">
  <h3>Soy el componente BINDINGS y vamos hacer un formulario</h3>

  <h5>formulario</h5>
  <form>
    <span>nombre</span>
    <input
      on:keyup={handleChange}
      name="nombre"
      type="text"
      bind:value={persona.nombre.value}
    />
    {#if persona.nombre.error && persona.nombre.value}
      <p class="error">{persona.nombre.errorInfo}</p>
    {/if}
    <br />
    <span>email</span>
    <input
      on:keyup={handleChange}
      name="email"
      type="text"
      bind:value={persona.email.value}
    />
    {#if persona.email.error && persona.email.value}
      <p class="error">hay un error</p>
    {/if}
    <br />

    <span>edad</span>
    <input
      on:keyup={handleChange}
      name="edad"
      type="number"
      bind:value={persona.edad.value}
    />
    <br />

    <span>SEXO :</span>
    <span>Masculino</span>
    <input
      on:change={handleChange}
      name="sexo"
      type="radio"
      bind:group={persona.sexo.value}
      value="Masculino"
    />
    <span>Femenino</span>
    <input
      on:change={handleChange}
      name="sexo"
      type="radio"
      bind:group={persona.sexo.value}
      value="Femenino"
    />
    <br />

    <span>pais</span>
    <select
      name="pais"
      on:change={handleChange}
      bind:value={persona.pais.value}
    >
      {#each paises as pais, i}
        <option value={pais}>
          {pais + i}
        </option>
      {/each}
    </select>
    <br />
    <div>
      <h4>Redes Sociales</h4>
      {#each redes_sociales as red}
        <input
          type="checkbox"
          bind:group={persona.redes_sociales.value}
          value={red}
          name="redes_sociales"
          on:change={handleChange}
        />
        <span>{red}</span>
      {/each}
    </div>
    <br />

    <div>
      <h4>Idiomas</h4>
      <select
        name="idiomas"
        on:change={handleChange}
        bind:value={persona.idiomas.value}
        multiple
      >
        {#each idiomas as idioma}
          <option value={idioma}>{idioma}</option>
        {/each}
      </select>
      <br />


<!--       {#each persona.idiomas.value ? persona.idiomas.value : [] as idioma}
        <label>
          <span>{idioma.idioma}</span>
          <span>{idioma.nivel}</span>
          <input type="range" bind:value={idioma.nivel} min="0" max="10" />
        </label>
      {/each} -->
    </div>
    <br />

    <label>
      <h5>Politicas</h5>
      <input type="checkbox" bind:checked={isAgree} />
      Estas de acuerdo con nuestras politicas?
    </label>
    <br />

    <button disabled={!isFormValid || !isAgree}>Enviar</button>
  </form>
  <h5>ESTADO DEL COMPONENTE</h5>
  <p>Persona: {JSON.stringify(persona)}</p>
  <p>isFormValid: {JSON.stringify(isFormValid)}</p>
</div>

<style>
  .body {
    background: rgb(52, 118, 116);
    color: black;
  }

  p.error {
    color: red;
  }
</style>
