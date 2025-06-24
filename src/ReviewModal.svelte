<script lang="ts">
  export let selectedPlace: string = '';
  export let onClose: () => void = () => {};
  let review: string = '';

  function submitReview() {
    const stored = JSON.parse(localStorage.getItem('reviews') || '{}');
    stored[selectedPlace] = stored[selectedPlace] || [];
    stored[selectedPlace].push(review);
    localStorage.setItem('reviews', JSON.stringify(stored));
    review = '';
    alert('レビューを投稿しました！');
    onClose();
  }
</script>

<div class="modal-overlay" on:click={onClose}>
  <div class="modal" on:click|stopPropagation>
    <h3 class="storename">{selectedPlace} へのレビュー</h3>
    <textarea bind:value={review} rows="4" cols="40" placeholder="レビューを入力してください"></textarea><br />
    <button on:click={submitReview}>投稿</button>
    <button on:click={onClose}>閉じる</button>
  </div>
</div>

<style>
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background: rgba(0,0,0,0.5);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 10000;
  }
  .modal {
    background: white;
    padding: 1rem;
    border-radius: 8px;
    max-width: 90%;
    box-shadow: 0 4px 10px rgba(0,0,0,0.3);
  }
  textarea {
    width: 100%;
  }
  button {
    margin-top: 0.5rem;
    margin-right: 0.5rem;
  }

  .storename{
    color:black;
    background-color: bisque;
  }
</style>
