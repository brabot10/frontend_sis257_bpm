<script setup lang="ts">
import { useRoute } from 'vue-router'
const location = useRoute()
import { useAuthStore } from '@/stores/index'
const authStore = useAuthStore()
</script>

<template>
  <header class="site-header">
    <div class="container">
      <div class="row">
        <div class="col-lg-12 col-12 d-flex flex-wrap">
          <p class="d-flex me-4 mb-0">
            <i class="bi-person custom-icon me-2"></i>
            <strong class="text-dark">Bienvenido a SIS257 Defensa Laboratorio</strong>
          </p>
        </div>
      </div>
    </div>
  </header>

  <!-- <nav class="navbar navbar-expand-lg"
    :style="location.path != '/' ? 'background-color: black !important' : ''"
  > -->
  <nav
    class="navbar navbar-expand-lg"
    :style="'background-color:' + (location.path != '/' ? 'black ' : '')"
  >
    <div class="container">
      <a class="navbar-brand" href="index.html"> QUE RICO! </a>
      <!-- <RouterLink to="/" class="navbar-brand"> QUE RICO!</RouterLink> -->

      <RouterLink v-if="!authStore.token" to="/login" class="btn custom-btn d-lg-none ms-auto me-4">
        Iniciar Sesión
      </RouterLink>

      <a v-else @click="authStore.logout()" class="btn custom-btn d-lg-none ms-auto me-4">Salir</a>

      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarNav"
        aria-controls="navbarNav"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav align-items-lg-center ms-auto me-lg-5">
          <li class="nav-item">
            <RouterLink to="/" class="nav-link click-scroll">Inicio</RouterLink>
          </li>
          <slot v-if="authStore.token">
            <li class="nav-item">
              <RouterLink to="/about" class="nav-link click-scroll">Menú</RouterLink>
            </li>
            <li class="nav-item">
              <RouterLink to="/platillos" class="nav-link click-scroll">Platillos</RouterLink>
            </li>
            <li class="nav-item">
              <RouterLink to="/pedido" class="nav-link click-scroll">Pedidos Realizados</RouterLink>
            </li>
            <li class="nav-item">
              <RouterLink to="/detalles" class="nav-link click-scroll">Detalles</RouterLink>
            </li>
            <li class="nav-item">
              <RouterLink to="/repartidor" class="nav-link click-scroll">Repartidor</RouterLink>
            </li>
            <li class="nav-item">
              <RouterLink to="/valoraciones" class="nav-link click-scroll">Valoraciones</RouterLink>
            </li>
            <li class="nav-item">
              <RouterLink to="/pago" class="nav-link click-scroll">Pagos</RouterLink>
            </li>
            <li class="nav-item">
              <RouterLink to="/usuarios" class="nav-link click-scroll">Usuarios</RouterLink>
            </li>
            <li class="nav-item">
              <a class="nav-link click-scroll" href="#">Hola {{ authStore.user }}</a>
            </li>
          </slot>
        </ul>
        <RouterLink
          v-if="!authStore.token"
          to="/login"
          class="btn custom-btn d-lg-block d-none custom-link"
          style="color: white"
        >
          Iniciar Sesión
        </RouterLink>

        <a v-else @click="authStore.logout()" class="btn custom-btn d-lg-block d-none" style="color: white">Salir</a>
      </div>
    </div>
  </nav>
</template>
