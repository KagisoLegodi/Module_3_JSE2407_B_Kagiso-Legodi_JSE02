<script>
    // @ts-ignore
    import { Link } from 'svelte-routing';
  
    export let products = [];
  
    let categories = [];
    let selectedCategory = '';
    let sortOrder = '';
  
    const fetchCategories = async () => {
      const res = await fetch('https://fakestoreapi.com/products/categories');
      categories = await res.json();
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
  
    fetchCategories();
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
  </style>
  
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
          <img src={product.image} alt={product.title} width="100" height="100" />
          <h2>{product.title}</h2>
          <p>${product.price}</p>
          <p>Category: {product.category}</p>
        </div>
      </Link>
    {/each}
  </div>
  