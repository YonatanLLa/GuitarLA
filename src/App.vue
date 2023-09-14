<script setup>
import { ref, reactive, onMounted } from "vue";
import { db } from "./data/guitarra";
import Guitarra from "./components/Guitarra.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

const guitarras = ref([]);
const carrito = ref([]);

onMounted(() => {
  guitarras.value = db;
});

const incrementar = (guitarra) => {
  const existCarr = carrito.value.findIndex(
    (producto) => producto.id === guitarra.id
  );
  console.log(existCarr);
  if (existCarr >= 0) {
    carrito.value[existCarr].cantidad++
  } else {
    guitarra.cantidad = 1;
    carrito.value.push(guitarra);
  }
};

console.log(guitarras);
</script>

<template>
  <Header :carrito="carrito" />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitarra
        v-for="guitarra in guitarras"
        :guitarra="guitarra"
        @incrementar="incrementar"
      />
    </div>
  </main>
  <Footer />
</template>

<style lang="scss" scoped></style>
