<template>
    <nav aria-label="Page navigation example">
      <ul class="pagination">
        <li class="page-item" :class="currentPage === 1 && 'disabled'"><a @click="paginate(currentPage - 1)" class="page-link" href="javascript:;">{{previous}}</a></li>
        <li class="page-item" :class="currentPage === data && 'active'" @click="paginate(data)" v-for="(data, index) in pageCount" :key="index"><a class="page-link" href="javascript:;">{{data}}</a></li>
        <li class="page-item" :class="currentPage === pageCount.length && 'disabled'"><a @click="paginate(currentPage + 1)" class="page-link" href="javascript:;">Next</a></li>
      </ul>
    </nav>
</template>

<script>
export default {
  name: 'Pagination',
  methods: {
    paginate(data){
        this.$emit('paginate-event', data)
    }
  },
  computed:{
    pageCount(){
      const pageNumbers = []
      for (let i = 1; i <= Math.ceil(this.totalPosts / this.postsPerPage); i++) {
        pageNumbers.push(i);
      }

      return pageNumbers;
    }
  },
  props: {
    totalPosts: {
      type: Number,
      required: true,
    },
    postsPerPage: {
      type: Number,
      required: true
    },
    currentPage: {
      type: Number,
      required: true
    },
    previous: {
      type: String,
      default: 'Previous'
    },
    next: {
      type: String,
      default: 'Next'
    },
  
  }
}
</script>