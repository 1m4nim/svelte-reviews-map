<script lang="ts">
  export let selectedPlace: string = '';
  let review: string = '';

  function submitReview() {
    const stored = JSON.parse(localStorage.getItem('reviews') || '{}');
    stored[selectedPlace] = stored[selectedPlace] || [];
    stored[selectedPlace].push(review);
    localStorage.setItem('reviews', JSON.stringify(stored));
    review = '';
    alert('レビューを投稿しました！');
  }
</script>

<h3 class="storename">{selectedPlace} へのレビュー</h3>
<textarea bind:value={review} rows="4" cols="40" placeholder="レビューを入力してください"></textarea><br />
<button on:click={submitReview}>投稿</button>
 
<style>
  .storename{
    background-color: bisque;
    color:black;
  }
</style>