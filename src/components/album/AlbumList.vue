<script setup lang="ts">
import type { Album } from '@/models/album'
import { onMounted, ref } from 'vue'
import http from '@/plugins/axios'
import router from '@/router'

const props = defineProps<{
  ENDPOINT_API: string
}>()

const ENDPOINT = props.ENDPOINT_API ?? ''
var albumes = ref<Album[]>([])

async function getalbumes() {
  albumes.value = await http.get(ENDPOINT).then((response) => response.data)
}

function toEdit(id: number) {
  router.push(`/albunes/editar/${id}`)
}

async function toDelete(id: number) {
  var r = confirm('¿Está seguro que se desea eliminar el Album?')
  if (r == true) {
    await http.delete(`${ENDPOINT}/${id}`).then(() => getalbumes())
  }
}

onMounted(() => {
  getalbumes()
})
</script>

<template>
  <div class="container">
    <nav aria-label="breadcrumb">
      <ol class="breadcrumb">
        <li class="breadcrumb-item"><RouterLink to="/">Inicio</RouterLink></li>
        <li class="breadcrumb-item active" aria-current="page">Albumes</li>
      </ol>
    </nav>

    <div class="row">
      <h2>Lista de Albumes</h2>
      <div class="col-12">
        <RouterLink to="/albunes/crear">
          <font-awesome-icon icon="fa-solid fa-plus" /> Crear Nuevo
        </RouterLink>
      </div>
    </div>

    <div class="table-responsive">
      <table class="table table-bordered">
        <thead>
          <tr>
            <th scope="col">N°</th>
            <th scope="col">Nombre</th>
            <th scope="col">Acciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(album, index) in albumes.values()" :key="album.id">
            <th scope="row">{{ index + 1 }}</th>
            <td>{{ album.nombre }}</td>
            <td>
              <button class="btn text-success" @click="toEdit(album.id)">
                <font-awesome-icon icon="fa-solid fa-edit" />
              </button>
              <button class="btn text-danger" @click="toDelete(album.id)">
                <font-awesome-icon icon="fa-solid fa-trash" />
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped></style>
