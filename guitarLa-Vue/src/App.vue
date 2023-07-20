<script setup>
import { db } from "./data/guitarras";
import { onMounted, ref } from "vue";
import Guitarra from "./components/Guitarra.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";
// import { onMounted } from 'vue';

// Reactive siempre es un objeto
/**
 * Si tienes un arreglo, es mejor usar reactive (datos agrupados)
 * Para cualquier otro tipo de dato es mejor ref
 */

// EJEMPLO DE REACTIVE
// const state = reactive({
//   guitarras: db
// })
// console.log(state.guitarras);

//EJEMPLO DE REf
// const guitar = ref([]);

// onMounted(() => {
//   guitar.value = db;
//   console.log('componente listo');
// })

const guitarras = ref([]);
const carrito = ref([]);

onMounted(() => {
  guitarras.value = db;
});

const incrementar = (guitarra) => {
  const existeCarrito = carrito.value.findIndex(
    (producto) => producto.id === guitarra.id
  );

  console.log(existeCarrito);

  if (existeCarrito >= 0) {
    carrito.value[existeCarrito].cantidad++;
  } else {
    guitarra.cantidad = 1;
    carrito.value.push(guitarra);
  }

  console.log("Agregando...");
  console.log(carrito.value);
};

const decrementarCantidad = (id) => {
  const index = carrito.value.findIndex((producto) => producto.id === id);
  if (carrito.value[index].cantidad <= 1) return;
  carrito.value[index].cantidad--;
  console.log("Menos...");
  console.log(id);
};

const incrementarCantidad = (id) => {
  const index = carrito.value.findIndex((producto) => producto.id === id);
  if (carrito.value[index].cantidad >= 50) return;
  carrito.value[index].cantidad++;
  console.log("Mas...");
  console.log(id);
};
</script>

<!-- * Estos son custom events o component events => :carrito="carrito"  -->
<!--TODO   : => Estoindica un prop  -->
<!--TODO   @ => Esto indica un evento -->

<template>
  <Header
    :carrito="carrito"
    @incrementar-cantidad="incrementarCantidad"
    @decrementar-cantidad="decrementarCantidad"
  />

  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>
    <div class="row mt-5">
      <!--** @ por que es un evento -->
      <Guitarra
        v-for="guitarra in guitarras"
        :guitarra="guitarra"
        @agregar-carrito="incrementar"
      />
      <!-- FIN GUITARRA -->
    </div>
  </main>

  <Footer />
</template>
