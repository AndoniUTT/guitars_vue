<script setup>
defineProps({
  carrito: {
    type: Array,
    required: true,
  },
  total: {
    type: Number,
    required: true,
  },
});

// Emitir eventos para que el componente padre maneje las acciones
const emit = defineEmits([
  "quita-uno",
  "agregar-carrito",
  "quita-guitarra",
  "vaciar-carrito",
]);
</script>

<template>
  <header class="py-5 header">
    <div class="container-xl">
      <div class="row justify-content-center justify-content-md-between">
        <div class="col-8 col-md-3">
          <a href="index.html">
            <img class="img-fluid" src="/img/logo.svg" alt="imagen logo" />
          </a>
        </div>
        <nav class="col-md-6 a mt-5 d-flex align-items-start justify-content-end">
          <div class="carrito">
            <img class="img-fluid" src="/img/carrito.png" alt="imagen carrito" />

            <div id="carrito" class="bg-white p-3">
              <p v-if="carrito.length === 0" class="text-center">
                El carrito está vacío
              </p>
              <div v-else>
                <table class="w-100 table">
                  <thead>
                    <tr>
                      <th>Imagen</th>
                      <th>Nombre</th>
                      <th>Precio</th>
                      <th>Cantidad</th>
                      <th></th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="guitar in carrito" :key="guitar.id">
                      <td>
                        <img
                          class="img-fluid"
                          :src="'/img/' + guitar.imagen + '.jpg'"
                          :alt="'imagen ' + guitar.nombre"
                        />
                      </td>
                      <td>{{ guitar.nombre }}</td>
                      <td class="fw-bold">
                        ${{ guitar.precio }}
                      </td>
                      <td class="d-flex align-items-center gap-2">
                        <!-- Botón para reducir cantidad -->
                        <button
                          type="button"
                          class="btn btn-dark"
                          @click="emit('quita-uno', guitar.id)"
                        >
                          -
                        </button>
                        {{ guitar.cantidad }}
                        <!-- Botón para aumentar cantidad -->
                        <button
                          type="button"
                          class="btn btn-dark"
                          @click="emit('agregar-carrito', guitar)"
                        >
                          +
                        </button>
                      </td>
                      <!-- Botón para eliminar la guitarra -->
                      <td>
                        <button
                          class="btn btn-danger"
                          type="button"
                          @click="emit('quita-guitarra', guitar.id)"
                        >
                          X
                        </button>
                      </td>
                    </tr>
                  </tbody>
                </table>

                <!-- Mostrar el total -->
                <p class="text-end">
                  Total a pagar: <span class="fw-bold">${{ total }}</span>
                </p>
                <!-- Botón para vaciar el carrito -->
                <button
                  class="btn btn-dark w-100 mt-3 p-2"
                  @click="emit('vaciar-carrito')"
                >
                  Vaciar Carrito
                </button>
              </div>
            </div>
          </div>
        </nav>
      </div>
      <!--.row-->

      <div class="row mt-5">
        <div class="col-md-6 text-center text-md-start pt-5">
          <h1 class="display-2 fw-bold">Modelo VAI</h1>
          <p class="mt-5 fs-5 text-white">
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Temporibus,
            possimus quibusdam dolor nemo velit quo, fuga omnis, iure molestias
            optio tempore sint at ipsa dolorum odio exercitationem eos inventore
            odit.
          </p>
          <p class="text-primary fs-1 fw-black">$399</p>
          <button
            type="button"
            class="btn fs-4 bg-primary text-white py-2 px-5"
            @click="emit('agregar-carrito', { id: 1, nombre: 'Modelo VAI', precio: 399, cantidad: 1, imagen: 'header_guitarra' })"
          >
            Agregar al Carrito
          </button>
        </div>
      </div>
    </div>

    <img class="header-guitarra" src="/img/header_guitarra.png" alt="imagen header" />
  </header>
</template>
