<script>
import axios from '@/axios'
import CommentForm from './CommentForm.vue'
import CommentList from './CommentList.vue'

export default {
  components: {
    CommentForm,
    CommentList
  },
  data() {
    return {
      film: {},
      comments: []
    }
  },
  created() {
    this.fetchFilm()
    this.fetchComments()
  },
  methods: {
    async fetchFilm() {
      const response = await axios.get(`/films/${this.$route.params.id}`)
      this.film = response.data
    },
    async fetchComments() {
      const response = await axios.get(`/films/${this.$route.params.id}/comments`)
      this.comments = response.data
    }
  }
}
</script>

<template>
  <div>
    <h1>{{ film.title }}</h1>
    <p>Release Date: {{ film.release_date }}</p>
    <h2>Comments</h2>
    <CommentForm :filmId="film.id" @commentAdded="fetchComments" />
    <CommentList :comments="comments" />
  </div>
</template>
