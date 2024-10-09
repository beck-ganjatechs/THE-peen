<script lang="ts">
  import { onMount } from 'svelte';
  import { gsap } from 'gsap';
  import LeadMagnet from './components/LeadMagnet.svelte';
  import SignUp from './components/SignUp.svelte';
  import InsuranceHealth from './components/InsuranceHealth.svelte';
  import Payment from './components/Payment.svelte';
  import Confirmation from './components/Confirmation.svelte';

  let currentStep = 0;
  const steps = [LeadMagnet, SignUp, InsuranceHealth, Payment, Confirmation];

  onMount(() => {
    gsap.from('#funnel-container', { opacity: 0, y: 50, duration: 1 });
  });

  function nextStep() {
    if (currentStep < steps.length - 1) {
      currentStep++;
      gsap.from('#funnel-container', { opacity: 0, x: 100, duration: 0.5 });
    }
  }

  function prevStep() {
    if (currentStep > 0) {
      currentStep--;
      gsap.from('#funnel-container', { opacity: 0, x: -100, duration: 0.5 });
    }
  }
</script>

<main class="bg-gray-100 min-h-screen flex items-center justify-center">
  <div id="funnel-container" class="bg-white p-8 rounded-lg shadow-lg max-w-md w-full">
    <h1 class="text-2xl font-bold mb-6 text-center">FC2 Evaluation</h1>
    <div class="mb-4 text-center text-sm text-gray-600">
      Step {currentStep + 1} of {steps.length}
    </div>
    <svelte:component this={steps[currentStep]} on:next={nextStep} on:prev={prevStep} />
    {#if currentStep > 0}
      <button on:click={prevStep} class="mt-4 w-full bg-gray-300 text-gray-700 p-2 rounded hover:bg-gray-400 transition-colors">
        Go Back
      </button>
    {/if}
  </div>
</main>