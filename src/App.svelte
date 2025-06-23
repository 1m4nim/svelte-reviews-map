<script>
  import Map from './Map.svelte';
  import ReviewForm from './ReviewForm.svelte';

  let selectedPlace = '';

  function handleSelectPlace(name) {
    selectedPlace = name;
  }
</script>

<main>
  <h1>レビュー地図</h1>
  <Map onSelectPlace={handleSelectPlace} />
  {#if selectedPlace}
    <ReviewForm selectedPlace={selectedPlace} />
  {/if}
</main>
