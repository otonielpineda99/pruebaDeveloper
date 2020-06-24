<template>
  <section class="single-post">
    <h1>{{post.title}}</h1>
    <p> by user  {{post.userId}} </p>
    <p> {{post.body}} </p>
    <hr />
    <h1> Comments </h1>
    <ul class="listado">
      <li class="comentario" v-for="comment of comments" :key="comment.id">
        <!--
          no le agregue el campo de fecha porque el api
          feik no proporciona ese campo -->
        <h3> {{ comment.name}} </h3>
        <p> {{comment.body}}</p>
      </li>
    </ul>
    <router-link to="/post">Dashboard</router-link>
  </section>
</template>
<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import axios from 'axios'

@Component
export default class SinglePost extends Vue {
  post: any = {};
  comments: Array<any> = []

  created () {
    this.postId()
  }

  postId (): Promise<void> {
    return axios.get(
      `https://jsonplaceholder.typicode.com/posts/${this.$route.params.id}/comments`
    )
      .then(response => {
        this.comments = response.data
      })
      .then(() => {
        return axios.get(`https://jsonplaceholder.typicode.com/posts/${this.$route.params.id}`)
          .then(response => (this.post = response.data))
      })
      .catch(error => console.error(error))
  }
}
</script>
<style lang="scss" scoped>
.single-post {
  text-align: left;

  .listado {
    list-style-type: none;
    padding: 0;
    width: 50%;
  }

  .comentario {
    border-bottom: 1px solid #010;
  }
}
</style>
