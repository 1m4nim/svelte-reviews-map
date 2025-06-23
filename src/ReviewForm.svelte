<script>
  export let selectedPlace = '';
  export let onClose = () => {};
  let review = '';

  // レビュー投稿処理
  function submitReview() {
    const stored = JSON.parse(localStorage.getItem('reviews') || '{}');
    stored[selectedPlace] = stored[selectedPlace] || [];
    stored[selectedPlace].push(review);
    localStorage.setItem('reviews', JSON.stringify(stored));
    review = '';
    alert('レビューを投稿しました！');
    onClose(); // 投稿後モーダルを閉じる
  }
</script>

<div class="modal-overlay" on:click={onClose}>
  <div class="modal" on:click|stopPropagation>
    <h3>{selectedPlace} へのレビュー</h3>
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
    width: 90%;
    max-width: 400px;
    box-shadow: 0 4px 12px rgba(0,0,0,0.3);
  }

  textarea {
    width: 100%;
    margin-bottom: 0.5rem;
    padding: 0.5rem;
    font-size: 1rem;
  }

  button {
    margin-right: 0.5rem;
    padding: 0.5rem 1rem;
    font-size: 1rem;
    cursor: pointer;
    border: none;
    border-radius: 4px;
  }

  button:first-of-type {
    background-color: #007bff;
    color: white;
  }

  button:first-of-type:hover {
    background-color: #0056b3;
  }

  button:last-of-type {
    background-color: #ccc;
  }

  button:last-of-type:hover {
    background-color: #999;
  }
</style>
