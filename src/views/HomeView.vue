
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
       
      </tbody>
    </table>
</div>
</template>
