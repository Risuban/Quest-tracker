<script>
  export let mission = { title: "", description: "", giver: "" };
  let isOpen = false;

  function toggle() {
    isOpen = !isOpen;
  }
</script>

<div class="box">
  <h2 class="title is-4" on:click={toggle}>
    {mission.title}
  </h2>

  {#if isOpen}
    <div class="content">
      <p><strong>Descripción:</strong> {mission.description}</p>
      <p><strong>Asignada por:</strong> {mission.giver}</p>
    </div>
  {/if}
</div>

<style>
  /* Aquí puedes agregar cualquier estilo adicional si es necesario */
</style>
