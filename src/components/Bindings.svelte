<script>
  let persona = {
    nombre: "",
    email: "",
    edad: "",
    sexo: "",
    pais: "",
    redes_sociales: "",
    idiomas: "",
  };

  $: isFormValid = validateForm(persona);
  let isAgree = false;
  let name;

  const paises = ["Estados Unidos", "Ecuador", "Peru", "Chile", "Argentina"];
  const redes_sociales = ["telegram", "instagram", "facebook", "whatsapp"];
  const idiomas = ["English", "Español"];

  // Usando la reactividad no tengo que usar handlekeyup para manejar directamente como en angular era.

  const validateForm = (object) => {
    for (const key in persona) {
      console.log(
        "hola",
        key,
        persona[key],
        !persona[key],
        typeof persona[key] === "array" && !persona[key].length > 0
      );
      if (
        persona[key] == "" ||
        (typeof persona[key] === "array" && !persona[key].length > 0)
      ) {
        console.log("puto retornando falso");
        return false;
      }
    }
    console.log("puto retornando true");
    return true;
  };

  const handleChange = (e) => {
    console.log("handlechange", e.target.value, persona.idiomas.value);
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
      bind:value={persona.nombre}
    />
    <br />
    <span>email</span>
    <input
      on:keyup={handleChange}
      name="email"
      type="text"
      bind:value={persona.email}
    />
    <br />

    <span>edad</span>
    <input
      on:keyup={handleChange}
      name="edad"
      type="number"
      bind:value={persona.edad}
    />
    <br />

    <span>SEXO :</span>
    <span>Masculino</span>
    <input
      on:change={handleChange}
      name="sexo"
      type="radio"
      bind:group={persona.sexo}
      value="Masculino"
    />
    <span>Femenino</span>
    <input
      on:change={handleChange}
      name="sexo"
      type="radio"
      bind:group={persona.sexo}
      value="Femenino"
    />
    <br />

    <span>pais</span>
    <select name="pais" on:change={handleChange} bind:value={persona.pais}>
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
          bind:group={persona.redes_sociales}
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
        bind:value={persona.idiomas}
        multiple
      >
        {#each idiomas as idioma}
          <option value={{ idioma, nivel: 5 }}>{idioma}</option>
        {/each}
      </select>
      <br />

      {#if persona.idiomas && persona.idiomas.length > 0}
        <h4>Elige el nivel del idioma:</h4>
      {/if}

      {#each persona.idiomas as idioma}
        <div>
          <p>Idioma:{idioma.idioma}</p>
          <span>Nivel:{idioma.nivel}</span>
          <input
            type="range"
            name="idiomas"
            on:change={handleChange}
            bind:value={idioma.nivel}
            min="0"
            max="10"
          />
        </div>
      {/each}
      <br />

      <!--       {#each persona.idiomas.value ? persona.idiomas.value : [] as idioma}
        <label>
          <span>{idioma.idioma}</span>
          <span>{idioma.nivel}</span>
          <input type="range" bind:value={idioma.nivel} min="0" max="10" />
        </label>
      {/each} -->

      <br />

      <label>
        <h5>Politicas</h5>
        <input type="checkbox" bind:checked={isAgree} />
        Estas de acuerdo con nuestras politicas?
      </label>
      <br />

      <button disabled={!isFormValid || !isAgree}>Enviar</button>
    </div>
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
