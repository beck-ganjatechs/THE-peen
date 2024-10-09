<script lang="ts">
  import { createEventDispatcher } from 'svelte';
  import { gsap } from 'gsap';
  import { onMount } from 'svelte';

  const dispatch = createEventDispatcher();

  let gender = '';
  let dob = '';
  let heightFeet = '';
  let heightInches = '';
  let weight = '';
  let address = '';
  let isValid = false;

  onMount(() => {
    gsap.from('form', { opacity: 0, y: 20, duration: 0.5, stagger: 0.1 });
  });

  function validateForm() {
    isValid = gender && dob && heightFeet && heightInches && weight && address;
  }

  function handleSubmit() {
    if (isValid) {
      console.log({ gender, dob, height: `${heightFeet}'${heightInches}"`, weight, address });
      dispatch('next');
    }
  }
</script>

<form on:submit|preventDefault={handleSubmit} class="space-y-4">
  <div>
    <select bind:value={gender} on:change={validateForm} required class="w-full p-2 border rounded">
      <option value="">Gender Assigned at Birth</option>
      <option value="male">Male</option>
      <option value="female">Female</option>
      <option value="other">Other</option>
    </select>
  </div>
  <div>
    <input type="date" bind:value={dob} on:change={validateForm} required class="w-full p-2 border rounded" />
  </div>
  <div class="flex space-x-2">
    <input type="number" bind:value={heightFeet} on:input={validateForm} placeholder="Height (ft)" required class="w-1/2 p-2 border rounded" min="1" max="8" />
    <input type="number" bind:value={heightInches} on:input={validateForm} placeholder="Height (in)" required class="w-1/2 p-2 border rounded" min="0" max="11" />
  </div>
  <div class="flex items-center">
    <input type="number" bind:value={weight} on:input={validateForm} placeholder="Weight" required class="w-full p-2 border rounded" min="1" max="999" />
    <span class="ml-2">pounds</span>
  </div>
  <div>
    <textarea bind:value={address} on:input={validateForm} placeholder="Shipping Address" required class="w-full p-2 border rounded"></textarea>
  </div>
  <button type="submit" class="w-full bg-blue-500 text-white p-2 rounded hover:bg-blue-600 transition-colors" disabled={!isValid}>
    Submit & Move to Insurance Info
  </button>
</form>