<script lang="ts">
  import { createEventDispatcher } from 'svelte';
  import { gsap } from 'gsap';
  import { onMount } from 'svelte';

  const dispatch = createEventDispatcher();

  let group = '';
  let pcn = '';
  let bin = '';
  let memberId = '';
  let medicalConditions = '';
  let medications = '';
  let allergies = '';
  let nitrileReaction = true;
  let fc2Disclaimer = false;
  let providerQuestions = '';
  let isValid = false;

  onMount(() => {
    gsap.from('form', { opacity: 0, y: 20, duration: 0.5, stagger: 0.1 });
  });

  function validateForm() {
    isValid = group && pcn && bin && memberId && medicalConditions && medications && allergies && fc2Disclaimer;
  }

  function handleSubmit() {
    if (isValid) {
      console.log({ group, pcn, bin, memberId, medicalConditions, medications, allergies, nitrileReaction, fc2Disclaimer, providerQuestions });
      dispatch('next');
    }
  }
</script>

<form on:submit|preventDefault={handleSubmit} class="space-y-4">
  <p class="text-lg font-semibold mb-4">Grab your insurance card!</p>
  <div class="grid grid-cols-2 gap-2">
    <input type="text" bind:value={group} on:input={validateForm} placeholder="Group (up to 6 chars)" maxlength="6" required class="p-2 border rounded" />
    <input type="text" bind:value={pcn} on:input={validateForm} placeholder="PCN (up to 8 chars)" maxlength="8" required class="p-2 border rounded" />
    <input type="text" bind:value={bin} on:input={validateForm} placeholder="BIN (6 digits)" pattern="\d{6}" maxlength="6" required class="p-2 border rounded" />
    <input type="text" bind:value={memberId} on:input={validateForm} placeholder="Member ID" required class="p-2 border rounded" />
  </div>
  <textarea bind:value={medicalConditions} on:input={validateForm} placeholder="Medical Conditions (or type none)" required class="w-full p-2 border rounded"></textarea>
  <textarea bind:value={medications} on:input={validateForm} placeholder="Medications (or type none)" required class="w-full p-2 border rounded"></textarea>
  <textarea bind:value={allergies} on:input={validateForm} placeholder="Allergies (or type none)" required class="w-full p-2 border rounded"></textarea>
  <div class="flex items-center">
    <input type="checkbox" bind:checked={nitrileReaction} on:change={validateForm} id="nitrile" class="mr-2" />
    <label for="nitrile">I have never had a bad reaction to nitrile or nitrile-based products</label>
  </div>
  <div class="flex items-center">
    <input type="checkbox" bind:checked={fc2Disclaimer} on:change={validateForm} id="fc2" required class="mr-2" />
    <label for="fc2">I understand that the FC2 should not be used with a diaphragm, cervical cap, sponge, or NuvaRing.</label>
  </div>
  <textarea bind:value={providerQuestions} on:input={validateForm} placeholder="Do you have any other questions for your healthcare provider?" class="w-full p-2 border rounded"></textarea>
  <button type="submit" class="w-full bg-blue-500 text-white p-2 rounded hover:bg-blue-600 transition-colors" disabled={!isValid}>
    Submit and Continue to Payment
  </button>
</form>