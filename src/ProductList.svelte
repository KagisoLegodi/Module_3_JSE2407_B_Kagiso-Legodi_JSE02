<script>
    // @ts-nocheck
    
    import { Link } from 'svelte-routing';
    
    export let products = [];
    
    let categories = [];
    let selectedCategory = '';
    let sortOrder = '';
    let loading = true; // Initialize loading state
    
    const fetchCategories = async () => {
      try {
        const res = await fetch('https://fakestoreapi.com/products/categories');
        if (res.ok) {
          categories = await res.json();
        } else {
          console.error('Failed to fetch categories');
        }
      } catch (error) {
        console.error('Error fetching categories:', error);
      }
    };
    
    const fetchProducts = async () => {
      try {
        const res = await fetch('https://fakestoreapi.com/products');
        if (res.ok) {
          products = await res.json();
          loading = false; // Set loading to false once data is fetched
        } else {
          console.error('Failed to fetch products');
        }
      } catch (error) {
        console.error('Error fetching products:', error);
      }
    };
    
    const handleCategoryChange = (event) => {
      selectedCategory = event.target.value;
    };
    
    const handleSortChange = (event) => {
      sortOrder = event.target.value;
    };
    
    $: filteredProducts = products
      .filter(product => selectedCategory ? product.category === selectedCategory : true)
      .sort((a, b) => sortOrder === 'low-to-high' ? a.price - b.price : sortOrder === 'high-to-low' ? b.price - a.price : 0);
    
    onMount(() => {
      fetchCategories();
      fetchProducts();
    });
  </script>
  
  <style>
    .product-list {
      display: flex;
      flex-wrap: wrap;
      gap: 1rem;
    }
    .product-card {
      border: 1px solid #090808;
      padding: 1rem;
      border-radius: 8px;
      cursor: pointer;
      width: 200px;
      background-color: white;
    }
    .loading {
      text-align: center;
      padding: 2rem;
      font-size: 1.5rem;
      color: #888;
    }
  </style>
  
  {#if loading}
    <div class="loading">Loading products...</div>
  {:else}
    <select on:change={handleCategoryChange}>
      <option value=''>All Categories</option>
      {#each categories as category}
        <option value={category}>{category}</option>
      {/each}
    </select>
    
    <select on:change={handleSortChange}>
      <option value=''>Default</option>
      <option value='low-to-high'>Price: Low to High</option>
      <option value='high-to-low'>Price: High to Low</option>
    </select>
    
    <div class="product-list">
      {#each filteredProducts as product}
        <Link to="/product/{product.id}">
          <div class="product-card">
            <img src={product.image} alt={product.title} class="w-40 max-w-[300px] h-auto object-contain rounded-lg" />
            <h2>{product.title}</h2>
            <p>${product.price}</p>
            <p>Category: {product.category}</p>
            <p>Ratings: {product.rating.rate}</p>
            <p>Reviews: {product.rating.count}</p>
          </div>
        </Link>
      {/each}
    </div>
  {/if}
  