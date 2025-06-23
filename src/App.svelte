<script>
  import Map from './Map.svelte';
  import ReviewPanel from './ReviewModal.svelte'; // 旧 ReviewModal

  let selectedPlace = '';
  function handleSelectPlace(name) {
    selectedPlace = name;
  }
</script>

<div class="container">
  <div class="map-panel">
    <Map onSelectPlace={handleSelectPlace} />
  </div>
  <div class="review-panel">
    {#if selectedPlace}
      <ReviewPanel selectedPlace={selectedPlace} />
    {:else}
      <p>地図上の施設をクリックしてレビューを開始してください。</p>
    {/if}
  </div>
</div>

<style>
  .container {
    display: flex;
    height: 100vh;
  }

  .map-panel {
    flex: 2;
    padding: 1rem;
  }

  .review-panel {
    flex: 1;
    border-left: 1px solid #ccc;
    padding: 1rem;
    overflow-y: auto;
  }
</style>
