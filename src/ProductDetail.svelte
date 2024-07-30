<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
  
    export let selectedProduct;
  
    const closeModal = () => {
      dispatch('close');
    };
  </script>
  
  <style>
    .modal {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.5);
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .modal-content {
      background: white;
      padding: 2rem;
      border-radius: 8px;
      width: 300px;
    }
    .close-btn {
      position: absolute;
      top: 1rem;
      right: 1rem;
      cursor: pointer;
    }
  </style>
  
  <!-- svelte-ignore a11y-no-static-element-interactions -->
  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <div class="modal" on:click={closeModal}>
    <div class="modal-content" on:click|stopPropagation>
      <!-- svelte-ignore a11y-click-events-have-key-events -->
      <!-- svelte-ignore a11y-no-static-element-interactions -->
      <span class="close-btn" on:click={closeModal}>&times;</span>
      <img src={selectedProduct.image} alt={selectedProduct.title} width="100%" />
      <h2>{selectedProduct.title}</h2>
      <p>{selectedProduct.description}</p>
      <p>Rating: {selectedProduct.rating.rate} ({selectedProduct.rating.count} reviews)</p>
      <p>${selectedProduct.price}</p>
      <button on:click={() => alert('Add to cart functionality not yet implemented')}>Add to Cart</button>
    </div>
  </div>
  