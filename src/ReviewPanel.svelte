<script>
  export let selectedPlace = '';
  let review = '';

  function submitReview() {
    const stored = JSON.parse(localStorage.getItem('reviews') || '{}');
    stored[selectedPlace] = stored[selectedPlace] || [];
    stored[selectedPlace].push(review);
    localStorage.setItem('reviews', JSON.stringify(stored));
    review = '';
    alert('レビューを投稿しました！');
  }

  $: existingReviews = JSON.parse(localStorage.getItem('reviews') || '{}')[selectedPlace] || [];
</script>

<div class="review-panel">
  <h3>{selectedPlace} へのレビュー</h3>

  {#if existingReviews.length > 0}
    <ul class="review-list">
      {#each existingReviews as r}
        <li>{r}</li>
      {/each}
    </ul>
  {:else}
    <p class="no-review">まだレビューはありません。</p>
  {/if}

  <textarea bind:value={review} rows="4" placeholder="レビューを入力してください"></textarea><br />
  <button on:click={submitReview}>投稿</button>
</div>

<style>
  .review-panel {
    padding: 1rem;
    background: #f9f9f9;
    height: 100%;
    overflow-y: auto;
    box-shadow: -1px 0 5px rgba(0,0,0,0.1);
  }

  h3 {
    margin-top: 0;
    color: #333;
  }

  .review-list {
    padding-left: 1rem;
    margin-bottom: 1rem;
  }

  .review-list li {
    margin-bottom: 0.5rem;
    line-height: 1.4;
  }

  .no-review {
    color: #666;
    font-style: italic;
  }

  textarea {
    width: 100%;
    margin-bottom: 0.5rem;
    padding: 0.5rem;
    font-size: 1rem;
  }

  button {
    padding: 0.5rem 1rem;
    font-size: 1rem;
    background: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  button:hover {
    background: #0056b3;
  }
</style>
