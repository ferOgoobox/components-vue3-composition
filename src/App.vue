<script setup>
import {ref, computed, onMounted} from 'vue';

import BlogPost from './components/BlogPost.vue';
import PaginatePost from './components/PaginatePost.vue';
import LoadingSpinner from './components/LoadingSpinner.vue';

const posts = ref([]);
const favorito = ref('')
const postXpage = 10
const inicio = ref(0)
const fin = ref(postXpage)
const loading= ref(true)

// fetch('https://jsonplaceholder.typicode.com/posts')
//   .then(res => res.json())
//   .then(data => posts.value = data)
// .finally(() => {
//   loading.value = false
// })

// onMounted(async() => {
//   loading.value = true
//   try {
//     const res = await fetch('https://jsonplaceholder.typicode.com/posts')
//     posts.value = await res.json()
//   } catch (error) {
//     console.log(error)
//   } finally {
//     loading.value = false
//   }
// })

const fetchData =  async () => {
  try {
    const res = await fetch('https://jsonplaceholder.typicode.com/posts')
    posts.value = await res.json()
  } catch (error) {
    console.log(error)
  } finally {
    loading.value = false
  }
}
fetchData()

const cambiarFavorito = (post) => {
  favorito.value = post
}

const next = () => {
  inicio.value = inicio.value + postXpage
  fin.value = fin.value + postXpage
}
const previus = () => {
  inicio.value +=  - postXpage
  fin.value += - postXpage
}

const maxLength = computed(() => {
  return posts.value.length
})

</script>

<template>
  
  <LoadingSpinner v-if="loading"></LoadingSpinner>

  <div class="container" v-else>
    <h1>App</h1>
    <h2>Mis Post Favoritos: {{ favorito || 'No hay favorito' }}</h2>

    <PaginatePost 
      @nextNombre="next"
      @prevNombre="previus"
      :inicio="inicio"
      :fin="fin"
      :maxLength="maxLength"
      class="mt-2"
    >
    </PaginatePost>

    <BlogPost 
      v-for="(item) in posts.slice(inicio,fin)" 
      :key="item.id"
      :title= "item.title"
      :id= "item.id"
      :body = "item.body"
      @cambiarFavoritoNombre="cambiarFavorito"
      class = "mb-2 mt-2"
    ></BlogPost>
  </div>
</template>

<style>

</style>