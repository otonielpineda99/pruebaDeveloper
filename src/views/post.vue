<template>
  <section class="noticias">
    <h1>News</h1>
    <input v-model="filterPost"/>
    <ul class="listado">
      <li class="noticia" v-for="post of dataFilter()" :key="post.id">
        <router-link :to="`/singlepost/${post.id}`">
          <h1 class="titulo"> {{ post.title }} </h1>
        </router-link>
        <p> {{ post.body }} </p>
      </li>
    </ul>
  </section>
</template>
<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import axios from 'axios'

@Component
export default class Post extends Vue {
  posts: Array<any> = []
  filterPost = ''

  created () {
    this.listPost()
  }

  dataFilter () {
    if (!this.filterPost) {
      return this.posts
    }

    return this.posts.filter(item => {
      if (item.title.indexOf(this.filterPost) !== -1) {
        return item
      }
    })
  }

  listPost (): Promise<void> {
    return axios.get('https://jsonplaceholder.typicode.com/posts')
      .then(response => {
        this.posts = response.data
      })
      .catch(error => console.error(error))
  }
}
</script>
<style lang="scss" scoped>
  .noticias {
  text-align: left;
  padding-left: 100px;
  padding-right: 100px;

  .listado {
    list-style-type: none;
    padding: 0;
  }
  .noticia {
    padding: 20px;
    border-radius: 10px;
    box-shadow: 1px 1px 1px 1px rgba(0,0,0,0.5);
    margin-bottom: 10px;
  }

  .titulo {
    &:hover {
      color: lightblue;
    }
  }
}
</style>
