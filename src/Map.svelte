<script lang="ts">
  import { onMount } from 'svelte';
  import L from 'leaflet';
  import 'leaflet/dist/leaflet.css';
  export let onSelectPlace: (place: string) => void = () => {};
  let map;
  let keyword = '';
  let searchMarkers = [];

  function clearSearchMarkers() {
    searchMarkers.forEach(marker => map.removeLayer(marker));
    searchMarkers = [];
  }

  async function searchLocation() {
    if (!keyword) return;
    const res = await fetch(`https://nominatim.openstreetmap.org/search?q=${encodeURIComponent(keyword)}&format=json&limit=30`);
    const data = await res.json();
    if (data.length === 0) {
      alert('場所が見つかりませんでした。');
      clearSearchMarkers();
      return;
    }
    clearSearchMarkers();
    const firstPlace = data[0];
    map.setView([parseFloat(firstPlace.lat), parseFloat(firstPlace.lon)], 14);
    data.forEach(place => {
      const lat = parseFloat(place.lat);
      const lon = parseFloat(place.lon);
      const displayName = place.display_name;
      const marker = L.marker([lat, lon]).addTo(map);
      marker.bindPopup(`
        <strong>${displayName}</strong><br />
        <button id="review-btn-${lat}-${lon}" style="margin-top:5px;">この場所にレビューを書く</button>
      `);
      marker.on('popupopen', () => {
        const btn = document.getElementById(`review-btn-${lat}-${lon}`);
        if (btn) {
          btn.addEventListener('click', () => {
            onSelectPlace(displayName);
          });
        }
      });
      searchMarkers.push(marker);
    });
  }

  onMount(() => {
    map = L.map('map').setView([35.6812, 139.7671], 14);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png').addTo(map);
  });
</script>

<style>
  #map {
    height: 100%;
  }
  .search-bar {
    margin-bottom: 1rem;
  }
</style>

<div class="search-bar">
  <input type="text" bind:value={keyword} placeholder="地名を入力（例：渋谷駅）" />
  <button on:click={searchLocation}>検索</button>
</div>
<div id="map"></div>