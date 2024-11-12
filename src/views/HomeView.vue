
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
            <select v-model="filter.orderBy" class="form-select" @change="orderChange">
              <option value="asc">ASC</option>
              <option value="desc">DESC</option>
            </select>
          </div>
        </div>
        
        
      </div>
    </div>

    <div v-if="showModal">
      <Modal :item="item" ref="modalRef" />
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
            <h5 class="card-title" style="cursor: pointer;"  @click="openModal(item)">
              {{ item.name.official }}
            </h5>
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
      <div class="col-12">
        <nav aria-label="Page navigation example">
          <ul class="pagination">
            <!-- Previous Button -->
            <li class="page-item" :class="{ disabled: filter.currentPage === 1 }">
              <a class="page-link" href="#" @click.prevent="goToPage(filter.currentPage - 1)">Previous</a>
            </li>

            <!-- Page Numbers -->
            <li 
              v-for="page in visiblePages" 
              :key="page" 
              class="page-item" 
              :class="{ active: page === filter.currentPage }">
              <a class="page-link" href="#" @click.prevent="goToPage(page)">{{ page }}</a>
            </li>

            <!-- Next Button -->
            <li class="page-item" :class="{ disabled: filter.currentPage === filter.totalPages }">
              <a class="page-link" href="#" @click.prevent="goToPage(filter.currentPage + 1)">Next</a>
            </li>
          </ul>

        </nav>
      </div>
    </div>
</div>
</template>
<script setup>
import { onMounted, ref, reactive, computed, nextTick } from 'vue'
import Modal from '@/components/Modal.vue'
import axios from 'axios'

const showModal = ref(false)
const modalRef = ref(null)
const item = reactive({})
const filter = reactive({
  pageSize: 25,
  currentPage: 1,
  totalPages: 0,
  search: '',
  orderBy: 'desc'
})
const lists = ref([])
const all_data = ref([])


//mounted for hook when initial page
onMounted(() => {
  get()
  
})

//computed
const visiblePages = computed(() => {
  const pages = [];
  const half = Math.floor(3 / 2); // 1 page before and 1 page after current
  const startPage = Math.max(1, filter.currentPage - half);
  const endPage = Math.min(filter.totalPages, filter.currentPage + half);

  // Ensure that we have exactly 3 pages (current + 1 before, 1 after)
  for (let i = startPage; i <= endPage; i++) {
    pages.push(i);
  }

  return pages;
})


//function 
const get = () => {
  axios.get("https://restcountries.com/v3.1/all?fields=name,flags,cca2,cca3,altSpellings,idd").then(response => {
    all_data.value = response.data
    getPaginatedData(all_data.value, filter.currentPage)
    filter.totalPages = Math.ceil(all_data.value.length / filter.pageSize);
  })
}

const orderChange = () => {
  return lists.value.sort((a, b) => {
    const aName = a.name.official;
    const bName = b.name.official;

    if (aName < bName) return filter.orderBy === 'asc' ? -1 : 1;
    if (aName > bName) return filter.orderBy === 'asc' ? 1 : -1;
    return 0; // Equal values
  });
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
  orderChange()
}

const openModal = (data) => {
  showModal.value = true
  Object.assign(item, data)
  nextTick(() => {
    modalRef.value.showModalAction()
  })
  
}


const goToPage = (page) => {
  if (page < 1 || page > filter.totalPages) return;
  filter.currentPage = page;
  getPaginatedData(all_data.value, filter.currentPage)
}


</script>