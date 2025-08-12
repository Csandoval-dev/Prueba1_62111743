<script setup>
  import { ref, onMounted, watch } from 'vue'
  import { db } from './data/articulos'
  import Articulo from './components/Articulo.vue'
  import Header from './components/Header.vue'
  import Footer from './components/Footer.vue'

  const articulos = ref([])
  const carrito = ref([])
  const articulo = ref({})

  watch(carrito, () => {
    guardarLocalStorage()
  }, {
    deep: true
  })

  const guardarLocalStorage = () => {
    localStorage.setItem('carrito', JSON.stringify(carrito.value))
  }

  onMounted(() => {
    articulos.value = db
    articulo.value = db[0]
  })

  const agregarCarrito = (articulo) => {
    const existeCarrito = carrito.value.findIndex(producto => 
      producto.id === articulo.id
    )
    
    if (existeCarrito >= 0) {
      carrito.value[existeCarrito].cantidad++
    } else {
      articulo.cantidad = 1
      carrito.value.push(articulo)
    }
    guardarLocalStorage()
  }

  const decrementarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id === id)
    if (carrito.value[index].cantidad <= 1) return 
    carrito.value[index].cantidad--
  }

  const incrementarCantidad = (id) => {
    const index = carrito.value.findIndex(producto => producto.id === id)
    carrito.value[index].cantidad++
  }
</script>

<template>
   <!-- Renderización del Header -->
   <Header 
     :carrito="carrito"
     :articulo="articulo"
     @decrementar-cantidad="decrementarCantidad"
     @incrementar-cantidad="incrementarCantidad"
     @agregar-carrito="agregarCarrito" 
   />

  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>
    <div class="row mt-5">
      <!-- Renderización de Artículos -->
      <Articulo 
        v-for="articulo in articulos"
        :key="articulo.id"
        :articulo="articulo" 
        @agregar-carrito="agregarCarrito"
      />
    </div>
  </main>

  <!-- Renderización del Footer -->
  <Footer/>
</template>

<style scoped>
h1 {
  text-transform: uppercase;
  color: green;
}
</style>
