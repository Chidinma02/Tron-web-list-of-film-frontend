<!-- <script>
import axios from '@/axios'
export default {
  data() {
    return {
      films: []
    }
  },
  created() {
    this.fetchFilms()
  },
  methods: {
    async fetchFilms() {
      const response = await axios.get('/films')
      this.films = response.data
    }
  }
}
</script>

<template>
  <div>
    <h1>Films</h1>
    <table class="table table-bordered films-table">
      <thead>
        <tr>
          <th>Title</th>
          <th>Release Date</th>
          <th>Comments Count</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="film in films" :key="film.id">
          <td>
            <router-link :to="{ name: 'Film', params: { id: film.id } }">
            {{ film.title }}
             </router-link> -->
<!-- </td>
          <td>{{ film.release_date }}</td>
          <td>{{ film.comments_count }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<style scoped> -->
<!-- .films-table {
      margin-top: 20px;
    }
    .films-table th, .films-table td {
      text-align: center;
    }
    .films-table th {
      background-color: #f8f9fa;
    }
</style> -->
<template>
  <div>
    <h1>Films</h1>
    <table class="table table-bordered films-table">
      <thead>
        <tr>
          <th>Title</th>
          <th>Release Date</th>
          <th>Comments Count</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="film in films" :key="film.id">
          <td>{{ film.title }}</td>
          <td>{{ film.release_date }}</td>
          <td>{{ film.comments_count }}</td>
          <td>
            <button @click="showComments(film)">View Comments</button>
            <button @click="showAddCommentModal(film)">Add Comment</button>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Comments Modal -->
    <div v-if="showCommentsModal" class="modal">
      <div class="modal-content">
        <span class="close" @click="closeCommentsModal">&times;</span>
        <h2>Comments for {{ selectedFilm.title }}</h2>
        <ul>
          <li v-for="comment in comments" :key="comment.id">{{ comment.comment }}</li>
        </ul>
      </div>
    </div>

    <!-- Add Comment Modal -->
    <div v-if="showAddCommentModalFlag" class="modal">
      <div class="modal-content">
        <span class="close" @click="closeAddCommentModal">&times;</span>
        <h2>Add Comment to {{ selectedFilm.title }}</h2>
        <form @submit.prevent="addComment">
          <textarea v-model="newComment" maxlength="500" required></textarea>
          <button type="submit">Add Comment</button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      films: [],
      selectedFilm: null,
      comments: [],
      newComment: '',
      showCommentsModal: false,
      showAddCommentModalFlag: false
    }
  },
  created() {
    this.fetchFilms()
  },
  methods: {
    async fetchFilms() {
      try {
        const response = await axios.get('http://localhost:8000/api/films')
        this.films = response.data
      } catch (error) {
        console.error('Error fetching films:', error)
      }
    },
    async showComments(film) {
      this.selectedFilm = film
      try {
        const response = await axios.get(`http://localhost:8000/api/films/${film.id}/comments`)
        this.comments = response.data
        this.showCommentsModal = true
      } catch (error) {
        console.error('Error fetching comments:', error)
      }
    },
    showAddCommentModal(film) {
      this.selectedFilm = film
      this.showAddCommentModalFlag = true
    },
    async addComment() {
      if (this.newComment.trim() === '') return
      try {
        await axios.post(`http://localhost:8000/api/films/${this.selectedFilm.id}/comments`, {
          comment: this.newComment
        })
        this.newComment = ''
        this.showAddCommentModalFlag = false
        this.fetchFilms() // Refresh films list to update comments count
      } catch (error) {
        console.error('Error adding comment:', error)
      }
    },
    closeCommentsModal() {
      this.showCommentsModal = false
      this.selectedFilm = null
    },
    closeAddCommentModal() {
      this.showAddCommentModalFlag = false
      this.selectedFilm = null
    }
  }
}
</script>

<style scoped>
.films-table {
  margin-top: 20px;
}
.films-table th,
.films-table td {
  text-align: center;
}
.films-table th {
  background-color: #f8f9fa;
}
.modal {
  display: block;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgb(0, 0, 0);
  background-color: rgba(0, 0, 0, 0.4);
  padding-top: 60px;
}
.modal-content {
  background-color: #fefefe;
  margin: 5% auto;
  padding: 20px;
  border: 1px solid #888;
  width: 80%;
}
.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}
.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}
</style>
