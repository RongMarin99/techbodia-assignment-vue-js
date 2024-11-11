
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
          <th scope="col">First</th>
          <th scope="col">Last</th>
          <th scope="col">Handle</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th scope="row">1</th>
          <td>Mark</td>
          <td>Otto</td>
          <td>@mdo</td>
        </tr>
        <tr>
          <th scope="row">2</th>
          <td>Jacob</td>
          <td>Thornton</td>
          <td>@fat</td>
        </tr>
        <tr>
          <th scope="row">3</th>
          <td colspan="2">Larry the Bird</td>
          <td>@twitter</td>
        </tr>
      </tbody>
    </table>
</div>
</template>
