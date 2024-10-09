<script lang="ts">
  import { createEventDispatcher } from 'svelte';
  import { gsap } from 'gsap';
  import { onMount } from 'svelte';

  const dispatch = createEventDispatcher();

  let firstName = '';
  let lastName = '';
  let email = '';
  let phone = '';
  let isValid = false;

  onMount(() => {
    gsap.from('form', { opacity: 0, y: 20, duration: 0.5, stagger: 0.1 });
  });

  function validateForm() {
    isValid = firstName && lastName && email && phone && /^\(\d{3}\) \d{3}-\d{4}$/.test(phone);
  }

  function formatPhone() {
    const cleaned = phone.replace(/\D/g, '');
    const match = cleaned.match(/^(\d{3})(\d{3})(\d{4})$/);
    if (match) {
      phone = `(${match[1]}) ${match[2]}-${match[3]}`;
    }
    validateForm();
  }

  function handleSubmit() {
    if (isValid) {
      console.log({ firstName, lastName, email, phone });
      dispatch('next');
    }
  }
</script>

<form on:submit|preventDefault={handleSubmit} class="space-y-4">
  <div>
    <input type="text" bind:value={firstName} on:input={validateForm} placeholder="First Name" required class="w-full p-2 border rounded" />
  </div>
  <div>
    <input type="text" bind:value={lastName} on:input={validateForm} placeholder="Last Name" required class="w-full p-2 border rounded" />
  </div>
  <div>
    <input type="email" bind:value={email} on:input={validateForm} placeholder="Email" required class="w-full p-2 border rounded" />
  </div>
  <div>
    <input type="tel" bind:value={phone} on:input={formatPhone} placeholder="Phone (xxx) xxx-xxxx" required class="w-full p-2 border rounded" />
  </div>
  <button type="submit" class="w-full bg-blue-500 text-white p-2 rounded hover:bg-blue-600 transition-colors" disabled={!isValid}>
    Let's Continue Your FC2 Evaluation
  </button>
</form>