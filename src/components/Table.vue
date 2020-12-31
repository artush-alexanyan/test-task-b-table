<template>
    <div class="table">
      <div class="container">
          <h3 class="text-center text-secondary mt-5">
              Table Component
          </h3>
          <label for="" class="mt-5">Search for comment name</label>
          <input type="text" class="form-control mt-3" v-model="searchText">
          <div class="overflow-auto mt-5">
            <div v-if="searchByName.length > 0">
              <b-table
                id="my-table"
                :items="searchByName"
                :per-page="perPage"
                :current-page="currentPage"
                small
              ></b-table>              
            </div>
            <div v-else>
                <h5 class="text-center text-info">No items found</h5>
            </div>
            <b-pagination
              v-model="currentPage"
              :total-rows="rows"
              :per-page="perPage"
              aria-controls="my-table"
              class="mt-5"
            ></b-pagination>            
          </div>          
      </div>
    </div>
</template>

<script>
import api from '../api'

export default {
  name: 'Table',
  data: () => ({
      perPage: 3,
      currentPage: 1,
      comments: [],
      searchText: ''
  }),
  computed: {
    rows() {
      return this.searchByName.length
    },
    searchByName () {
      let tempCommenst = this.comments

      if(this.searchText !== "" && this.searchText){
          tempCommenst = tempCommenst.filter(item => {
          return item.Name
            .toUpperCase()
            .includes(this.searchText.toUpperCase())            
          })
      }
    return tempCommenst
    }
  },  
  methods: {
    getComments () {
      api.get().then(res => {
        res.data.filter(comment => {
          this.comments.push({
            Id: comment.id,
            Name: comment.name,
            Email: comment.email,
            Body: comment.body
          })
        })
      })
    }
  },
  created () {
    this.getComments()
  }
}
</script>