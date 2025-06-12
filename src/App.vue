<script setup>

import { ref, reactive, onMounted } from 'vue';
import { db } from './data/guitarras.js';
import Guitarra from './components/Guitarra.vue';
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';

const guitarras = ref([]);
const carrito = ref([]);
const guitarra = ref({})


onMounted(() => {
  guitarras.value = db;
  guitarra.value = db[3]

});
const agregarCarrito = (guitarra) => {
  const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id);
  if (existeCarrito >= 0) {
    // Si el producto ya existe en el carrito, incrementamos la cantidad
    carrito.value[existeCarrito].cantidad++;
    return;
  } else {
    // Si el producto no existe, lo agregamos al carrito
    guitarra.cantidad = 1; // Aseguramos que la guitarra tenga una cantidad inicial
    carrito.value.push(guitarra);
  }

};

const decrementarCantidad = (id) => {
  // Lógica para decrementar la cantidad de un producto en el carrito
  // Aquí puedes implementar la lógica según tus necesidades
  const index = carrito.value.findIndex(producto => producto.id === id);
  if (carrito.value[index].cantidad <= 1) return; // Evita que la cantidad sea menor a 1
  carrito.value[index].cantidad--;

};

const incrementarCantidad = (id) => {
  // Lógica para incrementar la cantidad de un producto en el carrito
  // Aquí puedes implementar la lógica según tus necesidades
  const index = carrito.value.findIndex(producto => producto.id === id);
  if (carrito.value[index].cantidad >= 5) return; // Evita errores si el producto no se encuentra
  carrito.value[index].cantidad++;
  
};


const eliminarProducto = (id) => {
  // Lógica para eliminar un producto del carrito
  const index = carrito.value.findIndex(producto => producto.id === id);
  if (index !== -1) {
    carrito.value.splice(index, 1);
  }
};
const vaciarCarrito = () => {
  // Lógica para vaciar el carrito
  carrito.value.splice(0, carrito.value.length);
};


// const state = reactive({
//   guitarras: db
// });
// console.log(state.guitarras)

</script>

<template>

  <Header 
    :carrito="carrito"
    :guitarra="guitarra"
    @decrementarCantidad="decrementarCantidad"
    @incrementarCantidad="incrementarCantidad"
    @agregarCarrito="agregarCarrito"
    @eliminarProducto="eliminarProducto"
    @vaciarCarrito="vaciarCarrito"
  
  >
  </Header>

  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">

      <Guitarra v-for="guitarra in guitarras" :guitarra="guitarra" @agregarCarrito="agregarCarrito"></Guitarra>

    </div>
  </main>
  <Footer></Footer>

</template>
