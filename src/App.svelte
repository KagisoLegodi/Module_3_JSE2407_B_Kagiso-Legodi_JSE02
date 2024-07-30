<script>
  import { onMount } from 'svelte';
  import { Router, Route } from 'svelte-routing';
  import ProductList from './ProductList.svelte';
  import ProductDetail from './ProductDetail.svelte';
  import Header from './Header.svelte';
  import "./app.css";

  let products = [];
  let loading = true;

  const fetchProducts = async () => {
    const res = await fetch('https://fakestoreapi.com/products');
    products = await res.json();
    loading = false;
  };

  onMount(fetchProducts);
</script>

<style>
  .container {
    padding: 2rem;
  }
</style>

<Header />
<Router>
  <div class="container">
    {#if loading}
      <p>Loading...</p>
    {:else}
      <Route path="/">
        <ProductList {products} />
      </Route>
      <Route path="product/:id" let:params>
        <ProductDetail id={params.id} />
      </Route>
    {/if}
  </div>
</Router>
