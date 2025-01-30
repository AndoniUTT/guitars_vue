<script setup>
import { computed, ref, onMounted, watch } from "vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";
import Guitarra from "./components/Guitarra.vue";
import { db } from "./data/guitarras";

const guitars = ref(db);
const carrito = ref([]);

// Calcular el total del carrito
const total = computed(() =>
  carrito.value.reduce((t, g) => t + g.cantidad * g.precio, 0)
);

// Guardar el carrito en localStorage
function guardaStorage() {
  localStorage.setItem("carrito", JSON.stringify(carrito.value));
}

// Recuperar el carrito de localStorage
function recuperaStorage() {
  const datos = localStorage.getItem("carrito");
  return datos ? JSON.parse(datos) : [];
}

// Agregar un producto al carrito
function agregarCarrito(guitar) {
  const guitarId = carrito.value.findIndex((g) => g.id === guitar.id);
  if (guitarId === -1) {
    carrito.value.push({ ...guitar, cantidad: 1 });
  } else {
    carrito.value[guitarId].cantidad++;
  }
}

// Reducir la cantidad de un producto en el carrito
function quitaUno(id) {
  const guitarIndex = carrito.value.findIndex((g) => g.id === id);
  if (guitarIndex !== -1) {
    const guitar = carrito.value[guitarIndex];
    if (guitar.cantidad > 1) {
      carrito.value[guitarIndex].cantidad--;
    } else {
      carrito.value.splice(guitarIndex, 1);
    }
  }
}

// Quitar un producto completamente del carrito
function quitaGuitarra(id) {
  const guitarIndex = carrito.value.findIndex((g) => g.id === id);
  if (guitarIndex !== -1) {
    carrito.value.splice(guitarIndex, 1);
  }
}

// Vaciar el carrito
function vaciarCarrito() {
  carrito.value = [];
}

// Recuperar el carrito del almacenamiento al montar el componente
onMounted(() => {
  carrito.value = recuperaStorage();
});

// Observar cambios en el carrito y guardarlos en localStorage
watch(carrito, guardaStorage, { deep: true });
</script>

<template>
  <Header
    :carrito="carrito"
    :total="total"
    @agregar-carrito="agregarCarrito"
    @quita-uno="quitaUno"
    @quita-guitarra="quitaGuitarra"
    @vaciar-carrito="vaciarCarrito"
  />

  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitarra
        v-for="guitar in guitars"
        :key="guitar.id"
        :guitar="guitar"
        @agregar-carrito="agregarCarrito"
      />
    </div>
  </main>
  
  <Footer />
</template>
