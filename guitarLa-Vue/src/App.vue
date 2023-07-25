<script setup>
import { db } from "./data/guitarras";
import { onMounted, ref, watch } from "vue";
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
const guitarra = ref({});

watch(
  carrito,
  () => {
    guardarLocalStorage();
  },
  {
    deep: true,
  }
);

onMounted(() => {
  guitarras.value = db;
  guitarra.value = db[3];

  const carritoStorage = localStorage.getItem("carrito");
  if (carritoStorage) {
    carrito.value = JSON.parse(carritoStorage);
  }
});

const guardarLocalStorage = () => {
  localStorage.setItem("carrito", JSON.stringify(carrito.value));
};

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

  guardarLocalStorage();
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
  console.log(carrito.value);
};

const eliminarProducto = (id) => {
  carrito.value = carrito.value.filter((producto) => producto.id !== id);
};

const vaciarCarrito = () => {
  carrito.value = [];
};
</script>

<!-- * Estos son custom events o component events => :carrito="carrito"  -->
<!--TODO   : => Estoindica un prop  -->
<!--TODO   @ => Esto indica un evento -->

<template>
  <Header
    :carrito="carrito"
    :guitarra="guitarra"
    @incrementar-cantidad="incrementarCantidad"
    @decrementar-cantidad="decrementarCantidad"
    @agregar-carrito="incrementar"
    @eliminar-producto="eliminarProducto"
    @vaciar-carrito="vaciarCarrito"
  />

  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>
    <div class="row mt-5">
      <!--** @ por que es un evento -->
      <Guitarra
        v-for="guitarra in guitarras"
        :key="guitarra.id"
        :guitarra="guitarra"
        @agregar-carrito="incrementar"
      />
      <!-- FIN GUITARRA -->
    </div>
  </main>

  <Footer />
</template>
