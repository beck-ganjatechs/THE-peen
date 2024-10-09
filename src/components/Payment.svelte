<script lang="ts">
  import { createEventDispatcher } from 'svelte';
  import { gsap } from 'gsap';
  import { onMount } from 'svelte';

  const dispatch = createEventDispatcher();

  let billingAddress = '';
  let cardNumber = '';
  let expirationDate = '';
  let cvv = '';
  let nameOnCard = '';
  let insuranceAgreement = false;
  let idUpload = null;
  let selfieUpload = null;
  let signature = null;
  let isValid = false;

  onMount(() => {
    gsap.from('form', { opacity: 0, y: 20, duration: 0.5, stagger: 0.1 });
    // Assuming the shipping address is stored in localStorage
    billingAddress = localStorage.getItem('shippingAddress') || '';
  });

  function validateForm() {
    isValid = billingAddress && cardNumber && expirationDate && cvv && nameOnCard && insuranceAgreement && idUpload && selfieUpload && signature;
  }

  function handleSubmit() {
    if (isValid) {
      console.log({ billingAddress, cardNumber, expirationDate, cvv, nameOnCard, insuranceAgreement, idUpload, selfieUpload, signature });
      dispatch('next');
    }
  }
</script>

<form on:submit|preventDefault={handleSubmit} class="space-y-4">
  <textarea bind:value={billingAddress} on:input={validateForm} placeholder="Billing Address" required class="w-full p-2 border rounded"></textarea>
  <input type="text" bind:value={cardNumber} on:input={validateForm} placeholder="Card Number" required class="w-full p-2 border rounded" />
  <div class="flex space-x-2">
    <input type="text" bind:value={expirationDate} on:input={validateForm} placeholder="MM/YY" required class="w-1/2 p-2 border rounded" />
    <input type="text" bind:value={cvv} on:input={validateForm} placeholder="CVV" required class="w-1/2 p-2 border rounded" />
  </div>
  <input type="text" bind:value={nameOnCard} on:input={validateForm} placeholder="Name on Card" required class="w-full p-2 border rounded" />
  <div class="flex items-center">
    <input type="checkbox" bind:checked={insuranceAgreement} on:change={validateForm} id="insurance" required class="mr-2" />
    <label for="insurance">I authorize the pharmacy to coordinate with my medical provider and applicable insurance. I request that my order be billed to my insurance and shipped to my home.</label>
  </div>
  <div>
    <label for="id-upload" class="block mb-2">Upload Valid Driver's License or ID Card or Passport:</label>
    <input type="file" id="id-upload" on:change={validateForm} bind:files={idUpload} accept="image/*" required class="w-full p-2 border rounded" />
  </div>
  <div>
    <label for="selfie-upload" class="block mb-2">Upload a Selfie clearly showing your face:</label>
    <input type="file" id="selfie-upload" on:change={validateForm} bind:files={selfieUpload} accept="image/*" required class="w-full p-2 border rounded" />
  </div>
  <div>
    <label for="signature" class="block mb-2">Hand-drawn Signature:</label>
    <canvas id="signature" bind:this={signature} width="300" height="150" class="border rounded"></canvas>
    <button type="button" on:click={() => { signature.getContext('2d').clearRect(0, 0, 300, 150); validateForm(); }} class="mt-2 p-2 bg-gray-200 rounded">Clear Signature</button>
  </div>
  <div class="bg-gray-100 p-4 rounded">
    <h3 class="font-bold mb-2">Order Summary</h3>
    <img src="/path/to/fc2-image.jpg" alt="FC2 Female Condom" class="w-full mb-2 rounded" />
    <p>FC2 Female Condom (internal condom)</p>
    <p class="text-sm text-gray-600">Designed to be worn inside the vagina for protection against unintended pregnancy and the transmission of STIs.</p>
  </div>
  <button type="submit" class="w-full bg-blue-500 text-white p-2 rounded hover:bg-blue-600 transition-colors" disabled={!isValid}>
    Pay & Complete Registration
  </button>
</form>