<script setup lang="ts">
import { onMounted, ref } from 'vue'
import http from '@/plugins/axios'
import router from '@/router'
import type { Pedido } from '@/models/pedido'

var pedido = ref<Pedido[]>([])
async function getPedidos() {
  pedido.value = await http.get('pedido').then((response) => response.data) // del  view
}

onMounted(() => {
  getPedidos()
})
const props = defineProps<{
  ENDPOINT_API: string
}>()

const ENDPOINT = props.ENDPOINT_API ?? ''
const nombre = ref('')
const precio = ref('')
const idPedido = ref('')

async function crearPlatillo() {
  await http
    .post(ENDPOINT, {
      nombre: nombre.value,
      precio: precio.value,
      idPedido: idPedido.value
    })
    .then(() => router.push('/platillos'))
}

function goBack() {
  router.go(-1)
}
</script>

<template>
  <br /><br /><br />
  <div class="container">
    <nav aria-label="breadcrumb">
      <ol class="breadcrumb">
        <li class="breadcrumb-item"><RouterLink to="/">Inicio</RouterLink></li>
        <li class="breadcrumb-item">
          <RouterLink to="/platillos">Platillos</RouterLink>
        </li>
        <li class="breadcrumb-item active" aria-current="page" style="color: black">Crear Platillo</li>
      </ol>
    </nav>

    <div class="row">
      <h2>Crear Nuevo Platillo</h2>
    </div>

    <div class="row">
      <form @submit.prevent="crearPlatillo">
        <!--cuando yo aprete guardar me llma al metodo crearPlatillo-->
        <div class="form-floating mb-3">
          <input type="text" class="form-control" v-model="nombre" placeholder="Nombre" required />
          <label for="nombre">Nombre</label>
        </div>
        <div class="form-floating mb-3">
          <input
            type="number"
            class="form-control"
            v-model="precio"
            placeholder="Precio"
            required
          />
          <label for="precio">Precio</label>
        </div>
        <div class="form-floating mb-3">
          <select v-model="idPedido" class="form-select">
            <option v-for="pedidos in pedido" :value="pedidos.id">
              {{ pedidos.nombreC }}
            </option>
          </select>
          <label for="pedidos">Nombre del Cliente</label>
        </div>

        <div class="text-center mt-3">
          <button type="submit" class="btn btn-primary btn-lg">
            <font-awesome-icon icon="fa-solid fa-floppy-disk" /> Crear Platillo
          </button>
        </div>
      </form>
    </div>
    <div class="text-left">
      <button class="btn btn-link" @click="goBack">Volver</button>
    </div>
  </div>
</template>

<style></style>
