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

  const handleSubmit = async (e) => {
    console.log("entrando a submit", e.target, persona);
    try {
      const response = await fetch("http://localhost:9000/svelte-practice", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(persona),
      });
      console.log(response);
      const data = await response.json();
      console.log("DATA DEL SERVIDOR:", data);
    } catch (error) {
      console.log("ERROR :", error);
    }
  };
</script>

<div class="body">
  <h3>Soy el componente BINDINGS y vamos hacer un formulario</h3>

  <h5>formulario</h5>
  <form on:submit|preventDefault={handleSubmit}>
    <span>nombre</span>
    <input name="nombre" type="text" bind:value={persona.nombre} />
    <br />
    <span>email</span>
    <input name="email" type="text" bind:value={persona.email} />
    <br />

    <span>edad</span>
    <input name="edad" type="number" bind:value={persona.edad} />
    <br />

    <span>SEXO :</span>
    <span>Masculino</span>
    <input
      name="sexo"
      type="radio"
      bind:group={persona.sexo}
      value="Masculino"
    />
    <span>Femenino</span>
    <input
      name="sexo"
      type="radio"
      bind:group={persona.sexo}
      value="Femenino"
    />
    <br />

    <span>pais</span>
    <select name="pais" bind:value={persona.pais}>
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
        />
        <span>{red}</span>
      {/each}
    </div>
    <br />

    <div>
      <h4>Idiomas</h4>
      <select name="idiomas" bind:value={persona.idiomas} multiple>
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
            bind:value={idioma.nivel}
            min="0"
            max="10"
          />
        </div>
      {/each}

      <br />
      <br />

      <label>
        <h5>Politicas</h5>
        <input type="checkbox" bind:checked={isAgree} />
        Estas de acuerdo con nuestras politicas?
      </label>
      <br />

      <button type="submit" disabled={!isFormValid || !isAgree}>Enviar</button>
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
</style>
