<script>
  import { onMount } from 'svelte';
  import L from 'leaflet';
  import 'leaflet/dist/leaflet.css';

  export let onSelectPlace = (place) => {};

  let map;
  let keyword = '';

  // 検索結果のマーカーを複数管理
  let searchMarkers = [];

  // 既存のマーカーを全て削除する関数
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

    // 最初の場所に地図を移動
    const firstPlace = data[0];
    map.setView([parseFloat(firstPlace.lat), parseFloat(firstPlace.lon)], 14);

    data.forEach(place => {
      const lat = parseFloat(place.lat);
      const lon = parseFloat(place.lon);
      const displayName = place.display_name;

      // ポップアップ用のDOM要素を作成
      const popupContent = document.createElement('div');
      const title = document.createElement('strong');
      title.textContent = displayName;
      popupContent.appendChild(title);
      popupContent.appendChild(document.createElement('br'));

      const btn = document.createElement('button');
      btn.textContent = 'この場所にレビューを書く';
      btn.style.marginTop = '5px';
      btn.addEventListener('click', () => {
        onSelectPlace(displayName);
      });
      popupContent.appendChild(btn);

      const marker = L.marker([lat, lon]).addTo(map);
      marker.bindPopup(popupContent);

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
    height: 80vh;
    margin-top: 1rem;
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
