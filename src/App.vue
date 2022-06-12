<template>
  <div class="d-flex align-items-center justify-content-center min-vh-100" v-if="isLoading">
    <div class="spinner-border text-primary" role="status">
      <span class="visually-hidden">Loading...</span>
    </div>
    <div class="spinner-border" role="status">
      <span class="visually-hidden">Loading...</span>
    </div>
    <div class="spinner-border text-danger" role="status">
      <span class="visually-hidden">Loading...</span>
    </div>
    <div class="spinner-border text-warning" role="status">
      <span class="visually-hidden">Loading...</span>
    </div>
    <div class="spinner-border text-success" role="status">
      <span class="visually-hidden">Loading...</span>
    </div>
  </div>
  <div class="p-5 container" v-else>
    <div class="row">
        <div v-for="(photo, index) in paginatedPhotos" :key="index" class="col-lg-3 col-6">
          <div class="card">
            <img :src="photo.thumbnailUrl" class="card-img-top" alt="...">
            <div class="card-body">
              <h5 class="card-title">{{photo.title}}</h5>
            </div>
          </div>
        </div>
    </div>

    <div class="d-flex justify-content-center p-5">
      <Pagination previous="Prev" next="Next" :postsPerPage=postsPerPage :totalPosts="photos.length" :currentPage=currentPage @paginate-event="paginate" />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Pagination from "@/components/Pagination"
export default {
  name: 'App',
  components: {
    Pagination
  },
  data(){
    return {
      photos: [],
      currentPage: 1,
      postsPerPage: 8,
      isLoading: true,
    }
  },
  created(){
    axios.get('https://jsonplaceholder.typicode.com/albums/1/photos')
    .then((res) => {
      this.photos = res.data;
      this.isLoading = false
    })
    .catch((error) => {
      throw error
    })
  },
  methods: {
    paginate(pageNumber){
      this.currentPage = pageNumber;
    }
  },
  computed:{
    paginatedPhotos(){
      const indexOfLastPost = this.currentPage * this.postsPerPage;
      const indexOfFirstPost = indexOfLastPost - this.postsPerPage;
      const currentPosts = this.photos.slice(indexOfFirstPost, indexOfLastPost);
      return currentPosts
    }
  }
}
</script>
