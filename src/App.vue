<script setup>
import { ref, reactive, onMounted, watch } from "vue";
import { db } from "./data/guitarra";
import Guitarra from "./components/Guitarra.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

const guitarras = ref([]);
const carrito = ref([]);
const guitarra = ref({});

watch(carrito, ()=>{
  guardarLocalStorage()
}, {
  deep: true
})

onMounted(() => {
  guitarras.value = db;
  guitarra.value = db[3];
  const carritoStorage = localStorage.getItem('carrito')
  if (carritoStorage) {
    carrito.value = JSON.parse(carritoStorage)
  }
});

const guardarLocalStorage = () => {
  localStorage.setItem('carrito', JSON.stringify(carrito.value))
}

const agregarCarrito = (guitarra) => {
  const existCarr = carrito.value.findIndex(
    (producto) => producto.id === guitarra.id
  );
  if (existCarr >= 0) {
    carrito.value[existCarr].cantidad++;
  } else {
    guitarra.cantidad = 1;
    carrito.value.push(guitarra);
  }
  
};
const decrementarCantidad = (id) => {
  const index = carrito.value.findIndex((produc) => produc.id === id);

  if (carrito.value[index].cantidad <= 1) return;
  carrito.value[index].cantidad--;
  

};

const incrementarCantidad = (id) => {
  const indexI = carrito.value.findIndex((produc) => produc.id === id);
  carrito.value[indexI].cantidad++;
  

};

const deleteProduct = (id) => {
  carrito.value = carrito.value.filter((p) => {
    return p.id !== id;
    
  });
  
};

const vaciarCarrito = () => {
  carrito.value = []
  

};
</script>

<template>
  <Header
    :carrito="carrito"
    :guitarra="guitarra"
    @decrementarCantidad="decrementarCantidad"
    @incrementarCantidad="incrementarCantidad"
    @delete-product="deleteProduct"
    @agregar-carrito="agregarCarrito"
    @vaciar-carrito="vaciarCarrito"
  />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitarra
        v-for="guitarra in guitarras"
        :key="guitarra.id"
        :guitarra="guitarra"
        @incrementar="agregarCarrito"
      />
    </div>
  </main>
  <Footer />
</template>

<style lang="scss" scoped></style>
