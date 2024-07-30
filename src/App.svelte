<script>
  import { onMount } from 'svelte';
  // @ts-ignore
  import { Router } from 'svelte-routing';
  import ProductList from './ProductList.svelte';
  import ProductDetailModal from './ProductDetailModal.svelte';
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