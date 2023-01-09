<script setup>
import { ref, computed, onMounted } from "vue";

import BlogPost from './components/BlogPost.vue';
import PaginatePost from './components/PaginatePost.vue';
import LoadingSpinner from './components/LoadingSpinner.vue';

const posts = ref([]);
const postXpage = 10
const inicio =  ref(0)
const fin =  ref(postXpage)
const loading = ref(true)

const favorito = ref('')

const cambiarFavorito = (title) => {
  favorito.value = title
}

const next = () => {
  inicio.value = inicio.value + postXpage
  fin.value = fin.value + postXpage
}

const prev = () => {
  inicio.value += -postXpage
  fin.value += -postXpage
}

// onMounted(async() => {
//   loading.value = true
//   try {
//     const res = await fetch('https://jsonplaceholder.typicode.com/posts')
//     posts.value = await res.json()
//   } catch (error){
//     console.log(error)
//   } finally {
//     loading.value = false
//   }
// })

// fetch('https://jsonplaceholder.typicode.com/posts')
//     .then(res => res.json())
//     .then((data) => {
//       posts.value = data;
//     })
//     .catch(e => console.log(e))
//     .finally(() => {
//       loading.value = false
//       // setTimeout(() => {
//       //   loading.value = false
//       // }, 2000)
//     })
 
const fetchData = async() => {
  try {
    const res = await fetch('https://jsonplaceholder.typicode.com/posts')
    posts.value = await res.json()
  } catch (error){
    console.log(error)
  } finally {
    loading.value = false
  }
}

fetchData()

</script>

<template>
  <LoadingSpinner v-if="loading" />
  <div class="container">
    <h1 class="mt-2">APP</h1>
    <h2>Mi Post Favorito: {{ favorito }}</h2>

    <PaginatePost 
      @next="next" 
      @prev="prev" 
      :inicio="inicio" 
      :fin="fin" 
      :maxLength="posts.length"
      class="mb-2"
    />

    <BlogPost 
      v-for="post in posts.slice(inicio, fin)"
      :key="post.id"
      :title="post.title" 
      :id="post.id"
      :body="post.body" 
      @cambiarFavoritoNombre="cambiarFavorito"
      class="mb-2"  
    ></BlogPost>
  </div>
</template>