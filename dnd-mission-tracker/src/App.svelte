<script>
  import "bulma/css/bulma.css";
  import { onMount } from "svelte";
  import Papa from "papaparse";
  import MissionCard from "./MissionCard.svelte";

  let missions = [];
  let newMission = {
    title: "",
    description: "",
    giver: "",
  };

  let showModal = false; // Para controlar la visibilidad del modal

  // Función para cargar las misiones desde el archivo CSV
  onMount(() => {
    Papa.parse("/misiones.csv", {
      download: true,
      header: true,
      complete: (result) => {
        missions = result.data;
      },
    });
  });

  // Función para agregar una nueva misión
  function addMission() {
    if (newMission.title && newMission.description && newMission.giver) {
      // Agregar la nueva misión al array de misiones
      missions = [...missions, newMission];

      // Limpiar los campos del formulario
      newMission = { title: "", description: "", giver: "" };
      showModal = false; // Cerrar el modal
    } else {
      alert("Por favor, rellene todos los campos.");
    }
  }

  // Función para abrir el modal
  function openModal() {
    showModal = true;
  }

  // Función para cerrar el modal
  function closeModal() {
    showModal = false;
  }
</script>

<main class="section">
  <h1 class="title is-2 has-text-centered">Tracking de Misiones D&D</h1>

  <!-- Botón para abrir el formulario de creación de misión -->
  <div class="has-text-centered">
    <button class="button is-primary" on:click={openModal}
      >Agregar Nueva Misión</button
    >
  </div>

  <!-- Modal (tarjeta emergente) para agregar misión -->
  {#if showModal}
    <div class="modal is-active">
      <div class="modal-background" on:click={closeModal}></div>
      <div class="modal-content">
        <div class="box">
          <h2 class="title is-4">Nueva Misión</h2>

          <!-- Formulario de nueva misión -->
          <div class="field">
            <label class="label">Título</label>
            <div class="control">
              <input
                class="input"
                type="text"
                placeholder="Título de la misión"
                bind:value={newMission.title}
              />
            </div>
          </div>

          <div class="field">
            <label class="label">Descripción</label>
            <div class="control">
              <textarea
                class="textarea"
                placeholder="Descripción de la misión"
                bind:value={newMission.description}
              ></textarea>
            </div>
          </div>

          <div class="field">
            <label class="label">Asignada por</label>
            <div class="control">
              <input
                class="input"
                type="text"
                placeholder="Nombre del asignador"
                bind:value={newMission.giver}
              />
            </div>
          </div>

          <div class="control">
            <button class="button is-primary" on:click={addMission}
              >Agregar Misión</button
            >
            <button class="button is-light" on:click={closeModal}
              >Cancelar</button
            >
          </div>
        </div>
      </div>
      <button class="modal-close is-large" on:click={closeModal}></button>
    </div>
  {/if}

  <!-- Lista de misiones -->
  <div class="container">
    <h2 class="title is-4">Misiones Actuales</h2>
    <div>
      {#each missions as mission (mission.title)}
        <MissionCard {mission} />
      {/each}
    </div>
  </div>
</main>

<style>
  /* Estilos globales del modal */
  .modal.is-active {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .modal-background {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.6);
  }

  .modal-content {
    position: relative;
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    max-width: 500px;
    width: 100%;
  }

  .modal-close.is-large {
    position: absolute;
    top: 10px;
    right: 10px;
  }
</style>
