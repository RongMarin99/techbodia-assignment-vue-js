
<template>
  <div class="container">
    <div class="row p-3 bg-light mb-2 fixed-header" >
      <div class="col-4">
        <form class="d-flex" @submit.prevent="getPaginatedData(all_data, filter.currentPage, filter.search)">
          <input v-model="filter.search" class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
        </form>
      </div>
      <div class="col-8 d-flex justify-content-end align-items-center">
        <div class="row">
          <div class="col-8">
            <label class="mt-1" for="">Order by Country Name: </label>
          </div>
          <div class="col-4">
            <select class="form-select"  name="" id="">
              <option value="asc">ASC</option>
              <option  value="desc">DESC</option>
            </select>
          </div>
        </div>
        
        
      </div>
    </div>

    <!-- <table class="table">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Flags</th>
          <th scope="col">Country Name</th>
          <th scope="col">2 character Country Code</th>
          <th scope="col">3 character Country Code</th>
          <th scope="col">Native Country Name</th>
          <th scope="col">Alternative Country Name</th>
          <th scope="col">Country Calling Codes</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th scope="row">1</th>
          <td>Mark</td>
          <td>Otto</td>
          <td>@mdo</td>
          <td>Mark</td>
          <td>Otto</td>
          <td>@mdo</td>
          <td>@mdo</td>
        </tr>
      </tbody>
    </table> -->
    <div class="row">
      <div v-for="(item, index) in lists" class="col-3 mb-2">
        <div class="card">
          <div class="card-img-top image-bg" >
            <img  :src="item.flags.png" alt="">
          </div>
          
          <div class="card-body">
            <h5 class="card-title">{{ item.name.official }}</h5>
            <ul>
              <li>2 character Country Code: {{ item.cca2 }}</li>
              <li>3 character Country Code: {{ item.cca3 }}</li>
              <li>Native Country Name: {{ (item.name.nativeName[item.cca3.toLowerCase()])?.official ?? item.name.nativeName?.eng?.official }}</li>
              <li>Alternative Country Name: 
                <ul>
                    <li v-for="(row,i) in item.altSpellings" :key="i">
                      {{ row }}
                    </li>
                </ul>
              </li>
              <li>Country Calling Codes: {{ item.idd.root }}</li>
            </ul>
          </div>
        </div>
      </div>
    </div>

    <div class="row bg-light p-3">
      <div class="col-1">
        <select class="form-select" v-model="filter.pageSize">
          <option v-for="(item, index) in paginationOption" :key="index" :value="item">
            {{ item }}
          </option>
        </select>
      </div>
      <div class="col-11">
        <nav aria-label="Page navigation example">
          <ul class="pagination justify-content-end">
            <li class="page-item disabled">
              <a class="page-link" href="#" tabindex="-1" aria-disabled="true">Previous</a>
            </li>
            <li class="page-item"><a class="page-link" href="#">1</a></li>
            <li class="page-item"><a class="page-link" href="#">2</a></li>
            <li class="page-item"><a class="page-link" href="#">3</a></li>
            <li class="page-item">
              <a class="page-link" href="#">Next</a>
            </li>
          </ul>
        </nav>
      </div>
    </div>
</div>
</template>
<script setup>
import { onMounted, ref, reactive } from 'vue'
import axios from 'axios'

// const pageSize = ref(25)
// const currentPage = ref(1);
const filter = reactive({
  pageSize: 25,
  currentPage: 1,
  search: ''
})
const lists = ref([])
const all_data = ref([])
const paginationOption = ref([
  25, 50, 75, 100
])

//mounted for hook when initial page
onMounted(() => {
  get()
})


//function 
const get = () => {
  axios.get("https://restcountries.com/v3.1/all?fields=name,flags,cca2,cca3,altSpellings,idd").then(response => {
    all_data.value = response.data
    getPaginatedData(all_data.value, filter.currentPage)
  })
}



const getPaginatedData = (countries, page, searchTerm = '') => {
  if(searchTerm){
    lists.value = countries.filter(country => 
      country.name.official.toLowerCase().includes(searchTerm.toLowerCase())
    ); 
    return
  }

  const start = (page - 1) * filter.pageSize;
  const end = start + filter.pageSize;
  lists.value = countries.slice(start, end);
}
</script>