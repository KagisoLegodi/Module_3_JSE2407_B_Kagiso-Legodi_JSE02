<script>
  import { onMount } from 'svelte';

  export let params = {}; // Route parameters passed from the parent component
  let product = null;
  let productId = params.id || ''; // Extract productId from params

  const fetchProduct = async () => {
    try {
      const res = await fetch(`https://fakestoreapi.com/products/${productId}`);
      if (res.ok) {
        product = await res.json();
      } else {
        console.error('Failed to fetch product');
      }
    } catch (error) {
      console.error('Error fetching product:', error);
    }
  };

  onMount(() => {
    fetchProduct();
  });
</script>

<style>
  .product-detail {
    padding: 1.5rem;
    max-width: 800px;
    margin: 2rem auto;
    background-color: #f9f9f9;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
  .product-image {
    width: 100%;
    max-width: 500px;
    height: auto;
    object-fit: contain;
    border-radius: 8px;
    margin-bottom: 1rem;
  }
  h1 {
    font-size: 2rem;
    margin-bottom: 0.5rem;
    color: #333;
  }
  p {
    font-size: 1rem;
    margin: 0.5rem 0;
    color: #555;
  }
  .price {
    font-weight: bold;
    color: #1a202c;
  }
</style>

{#if product}
  <div class="product-detail">
    <img src={product.image} alt={product.title} class="product-image" />
    <h1>{product.title}</h1>
    <p class="price">${product.price}</p>
    <p>Category: {product.category}</p>
    <p>Ratings: {product.rating.rate} (Based on {product.rating.count} reviews)</p>
    <p>{product.description}</p>
  </div>
{:else}
  <p>Loading...</p>
{/if}
