<script setup lang="ts">
import { ref } from 'vue'

const API = 'https://api.github.com/users/'
const buscarusuario = ref('')
const resul = ref<GithubUser | null>(null)
const msjError = ref('')
const verJson = ref(false)
const showLoad = ref(false)

// Interfaz con propiedades
interface GithubUser {
  avatar_url: string
  login: string
  name: string
  bio: string
  html_url: string
  blog: string
}

async function doSearch() {
  //const response = await fetch(API + 'juanwmedia')

  try {
    showLoad.value = true
    resul.value = null
    msjError.value = ''

    const response = await fetch(API + buscarusuario.value)

    if (!response.ok) throw new Error('Usuario ' + buscarusuario.value + ' no localizado')

    const data: GithubUser = await response.json()
    resul.value = data
  } catch (error: unknown) {
    if (error instanceof Error) {
      msjError.value = error.message
    } else {
      msjError.value = String(error) // En caso de que no sea Error
    }
  } finally {
    buscarusuario.value = ''
    showLoad.value = false
  }
}
</script>

<template>
  <div class="PruebasApi">
    <!-- Search -->
    <div class="container">
      <h5>consultando API: {{ API }}</h5>

      <form class="search" v-on:submit.prevent="doSearch">
        <div class="input-group mb-3">
          <span class="input-group-text bg-dark text-warning" id="basic-addon1">@</span>
          <input
            v-model.trim="buscarusuario"
            type="text"
            class="form-control"
            required
            placeholder="escribe el nombre de usuario abuscar en GitHub"
          />
          <button class="btn btn-success" type="submit">
            <span
              v-show="showLoad"
              class="spinner-border spinner-border-sm"
              aria-hidden="true"
            ></span>
            <span class="visually-hidden" role="status">Buscando...</span>
            Buscar
          </button>
        </div>
      </form>

      <div v-if="resul" class="card text-bg-secondary mb-3">
        <div class="row g-0">
          <div class="col-4">
            <img v-bind:src="resul.avatar_url" class="img-fluid rounded-start" />
          </div>
          <div class="col-md-8">
            <div class="card-body">
              <h5 class="card-title">
                {{ resul.login }} <br />
                {{ resul.name }}
              </h5>
              <p class="card-text">{{ resul.bio }}</p>

              <a v-bind:href="resul.html_url" class="card-link" target="_blank">{{
                resul.html_url
              }}</a>
              <br />
              <a v-bind:href="resul.blog" class="card-link" target="_blank">{{ resul.blog }}</a>
            </div>
            <div class="card-footer">
              <div class="form-check form-switch">
                <input
                  v-model="verJson"
                  class="form-check-input"
                  type="checkbox"
                  id="checkNativeSwitch"
                />
                <label class="form-check-label" for="checkNativeSwitch"> Ver resultado json </label>
              </div>
            </div>
          </div>
        </div>
      </div>

      <pre v-if="verJson && resul">
        <code>{{ JSON.stringify(resul, null, 2) }}</code>
      </pre>

      <div class="alert alert-danger" role="alert" v-if="msjError">
        {{ msjError }}
      </div>
    </div>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .PruebasApi {
    padding-top: 40vh;
    min-height: 100vh;
    display: flexbox;
    align-items: center;
  }

  h5 {
    padding-bottom: 4px;
  }
}
</style>
