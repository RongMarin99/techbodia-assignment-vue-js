
<template>
  <div class="container">
    <div class="row p-3">
      <div class="col-4">
            <form class="d-flex">
              <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
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

    <table class="table">
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
    </table>

    <div class="row">
      <div class="col-1">
        <select class="form-select" v-model="pageSize">
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
import { onMounted, ref } from 'vue'
import axios from 'axios'

const pageSize = ref(25)
const currentPage = ref(1);
const lists = ref([])
const paginationOption = ref([
  25, 50, 75, 100
])

//mounted for hook when initial page
onMounted(() => {
  get()
})


//function 
const get = () => {
  axios.get("https://restcountries.com/v3.1/all?fields=name,flags").then(response => {
    lists.value = getPaginatedData(response.data, currentPage.value)
  })
}

const getPaginatedData = (countries, page) => {
  const start = (page - 1) * pageSize.value;
  const end = start + pageSize.value;
  return countries.slice(start, end);
}
</script>