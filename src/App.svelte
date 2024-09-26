<script>
  import { onMount } from 'svelte';
  import './app.css';  

  let services = [];
  let newService = { name: '', description: '', price: '' };
  let editingService = null;
  let errorMessage = '';

  onMount(() => {
    services = [
      { id: 1, name: 'General Checkup', description: 'Annual physical examination', price: 100 },
      { id: 2, name: 'Dental Cleaning', description: 'Professional teeth cleaning', price: 75 },
      { id: 3, name: 'Eye Exam', description: 'Comprehensive vision test', price: 50 },
    ];
  });

  function addService() {
    if (newService.name && newService.description && newService.price) {
      services = [...services, { ...newService, id: Date.now() }];
      newService = { name: '', description: '', price: '' };
      errorMessage = '';
    } else {
      errorMessage = 'All fields are required!';
    }
  }

  function deleteService(id) {
    services = services.filter(service => service.id !== id);
  }

  function startEditing(service) {
    editingService = { ...service };
  }

  function updateService() {
    if (editingService) {
      services = services.map(service => 
        service.id === editingService.id ? editingService : service
      );
      editingService = null;
    }
  }

  function cancelEditing() {
    editingService = null;
  }
</script>

<main class="container">
  <h1>Healthcare Services</h1>

  <!-- Add Service Section -->
  <div class="mb-8">
    <h2>Add New Service</h2>
    <form on:submit|preventDefault={addService} class="space-y-2">
      <input
        bind:value={newService.name}
        placeholder="Service Name"
      />
      <input
        bind:value={newService.description}
        placeholder="Description"
      />
      <input
        bind:value={newService.price}
        type="number"
        placeholder="Price"
      />
      <button type="submit" class="bg-blue-500">Add Service</button>
    </form>
    {#if errorMessage}
      <p style="color: red;">{errorMessage}</p>
    {/if}
  </div>

  <!-- List of Services Section -->
  <div>
    <h2>Service List</h2>
    {#each services as service (service.id)}
      <div class="service-list">
        {#if editingService && editingService.id === service.id}
          <form on:submit|preventDefault={updateService} class="space-y-2">
            <input
              bind:value={editingService.name}
            />
            <input
              bind:value={editingService.description}
            />
            <input
              bind:value={editingService.price}
              type="number"
            />
            <div class="flex space-x-2">
              <button type="submit" class="bg-green-500">Update</button>
              <button type="button" on:click={cancelEditing} class="bg-gray-500">Cancel</button>
            </div>
          </form>
        {:else}
          <h3 class="service-name">{service.name}</h3>
          <p class="service-description">{service.description}</p>
          <p class="service-price">${service.price}</p>
          <div class="flex space-x-2">
            <button on:click={() => startEditing(service)} class="bg-yellow-500">Edit</button>
            <button on:click={() => deleteService(service.id)} class="bg-red-500">Delete</button>
          </div>
        {/if}
      </div>
    {/each}
  </div>
</main>
