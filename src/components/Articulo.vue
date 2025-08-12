<script setup>
    const props = defineProps({
        articulo: {
            type: Object,
            required: true
        }
    })

    defineEmits(['agregar-carrito'])
</script>

<template>
    <div class="col-md-6 col-lg-4 my-4 row align-items-center">
        <div class="col-4">
            <img 
                class="img-fluid" 
                v-bind:src="'/img/' + articulo.imagen + '.jpg'" 
                :alt="'imagen articulo ' + articulo.nombre">
        </div>
        <div class="col-8">
            <h3 class="text-black fs-4 fw-bold text-uppercase">{{ articulo.nombre }}</h3>
           
            <p>{{ articulo.descripcion }}</p>
            <p class="fw-black text-primary fs-3">Lps.{{ articulo.precio }}</p>
            <button 
                type="button"
                class="btn btn-dark w-100"
                @click="$emit('agregar-carrito', articulo)"
            >Agregar al Carrito</button>
        </div>
    </div><!-- FIN ARTICULO -->
</template>

<style scoped>

</style>
