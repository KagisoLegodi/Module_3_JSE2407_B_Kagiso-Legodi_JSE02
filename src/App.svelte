<script>
  import { onMount } from 'svelte';
  import { Router, Route } from 'svelte-routing'; // Updated to import Route
  import ProductList from './ProductList.svelte';
  import ProductDetail from './ProductDetail.svelte'; // Renamed to ProductDetail to match the new component name
  import "./app.css";
  import Header from "./Header.svelte";

  let products = [];
  let selectedProduct = null;
  let showModal = false;

  const fetchProducts = async () => {
    const res = await fetch('https://fakestoreapi.com/products');
    const data = await res.json();
    products = data;
  };

  /**
   * @param {CustomEvent<{ id: number, title: string, price: number, description: string, category: string, image: string, rating: { rate: number, count: number } }>} event
   */
  const openModal = (event) => {
    const product = event.detail;
    selectedProduct = product;
    showModal = true;
  };

  const closeModal = () => {
    selectedProduct = null;
    showModal = false;
  };

  onMount(fetchProducts);
</script>

<style>
  .container {
    padding: 2rem;
  }
</style>

<Router>
  <Header />
  <div class="container">
    <Route path="/" let:params>
      <ProductList {products} on:select={openModal} />
    </Route>
    <Route path="/product/:id" let:params>
      <ProductDetail />
    </Route>
  </div>
</Router>
