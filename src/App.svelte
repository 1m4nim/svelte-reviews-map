<script lang="ts">
  import Map from './Map.svelte';
  import ReviewPanel from './ReviewPanel.svelte';
  import ReviewModal from './ReviewModal.svelte';
  import { onMount } from 'svelte';

  let selectedPlace = '';
  let isModalOpen = false;
  let windowWidth = 0;

  function handleSelectPlace(name: string) {
    selectedPlace = name;
    if (windowWidth < 768) {
      isModalOpen = true;
    }
  }

  function closeModal() {
    isModalOpen = false;
    selectedPlace = '';
  }

  function updateWidth() {
    windowWidth = window.innerWidth;
  }

  onMount(() => {
    updateWidth();
    window.addEventListener('resize', updateWidth);
    return () => window.removeEventListener('resize', updateWidth);
  });
</script>

<div class="container" class:is-narrow={windowWidth < 768}>
  <div class="map-panel">
    <Map onSelectPlace={handleSelectPlace} />
  </div>

  {#if windowWidth >= 768}
    <div class="review-panel">
      {#if selectedPlace}
        <ReviewPanel selectedPlace={selectedPlace} />
      {:else}
        <p>左の地図で場所を選んでください。</p>
      {/if}
    </div>
  {/if}
</div>

{#if isModalOpen}
  <ReviewModal selectedPlace={selectedPlace} onClose={closeModal} />
{/if}

<style>
  .container {
    display: flex;
    height: 100vh;
  }
  .map-panel {
    flex: 3;
    padding: 1rem;
    height: 100vh;
  }
  .review-panel {
    flex: 1;
    border-left: 1px solid #ccc;
    padding: 1rem;
    overflow-y: auto;
    height: 100vh;
  }
  .is-narrow {
    display: block;
    height: auto;
  }
</style>