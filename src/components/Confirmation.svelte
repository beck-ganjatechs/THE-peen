<script lang="ts">
  import { onMount } from 'svelte';
  import { gsap } from 'gsap';

  onMount(() => {
    gsap.from('.confirmation', { opacity: 0, y: 20, duration: 0.5 });
  });
</script>

<div class="confirmation text-center">
  <h2 class="text-2xl font-bold mb-4">Thank you!</h2>
  <p class="mb-4">Your information has been submitted, and a healthcare provider will review it shortly.</p>
  <p class="text-sm text-gray-600">You will receive a follow-up email with details of next steps and expected timelines.</p>
</div>