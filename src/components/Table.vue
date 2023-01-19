<template>
  <div type="container border" style="width: 70%">
    <table class="table">
      <thead class="table-dark">
        <tr>
          <th>ID</th>
          <th>FullName</th>
          <th>Age</th>
          <th>City</th>
          <th>State</th>
          <th>Country</th>
          <th>Diagnosis</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="u in listOfUsers" :key="u.id">
          <td>{{ u.id }}</td>
          <td>{{ u.fullName }}</td>
          <td>{{ u.age }}</td>
          <td>{{ u.city }}</td>
          <td>{{ u.state }}</td>
          <td>{{ u.country }}</td>
          <td>{{ u.diagnosis }}</td>
          <td>
            <i
              class="btn btn-outline-dark btn-sm bi bi-pencil"
              @click="editUser(u)"
              data-bs-toggle="modal"
              data-bs-target="#exampleModal"
            ></i>
            <i
              class="btn btn-outline-dark btn-sm mx-1 bi bi-trash"
              @click="deleteUser(u.id)"
            ></i>
          </td>
        </tr>
      </tbody>
    </table>
    <div
      class="modal fade"
      id="exampleModal"
      tabindex="-1"
      aria-labelledby="exampleModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h1 class="modal-title fs-5" id="exampleModalLabel">
              Edit Details
            </h1>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <label class="form-label">Full Name</label>
            <input class="form-control mb-2" v-model="newFullName" />

            <label class="form-label">Age</label>
            <input class="form-control mb-2" v-model="newAge" />

            <label class="form-label">City</label>
            <input class="form-control mb-2" v-model="newCity" />

            <label class="form-label">State</label>
            <input class="form-control mb-2" v-model="newState" />

            <label class="form-label">Country</label>
            <input class="form-control" v-model="newCountry" />

            <label class="form-label">Diagnosis</label>
            <input class="form-control" v-model="newDiagnosis" />
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-secondary"
              data-bs-dismiss="modal"
            >
              Close
            </button>
            <button type="button" class="btn btn-primary" @click="updateUser()">
              Save changes
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import Axios from "axios";
import { ref, onMounted } from "vue";
import Swal from "sweetalert2";

let listOfUsers = ref([]);
let id = ref(0);
let newFullName = ref("");
let newAge = ref();
let newCity = ref("");
let newState = ref("");
let newCountry = ref("");
let newDiagnosis = ref("");

const BASE_URL = "http://localhost:5282/Covid/";
onMounted(() => {
  loadUsers();
});

function loadUsers() {
  Axios.get(BASE_URL).then(function (response) {
    listOfUsers.value = response.data;
    console.log(listOfUsers.value, "mounted!");
  });
  console.log("this is executed at the start of the page");
}

function editUser(u) {
  id.value = u.id;
  newFullName.value = u.fullName;
  newAge.value = u.age;
  newCity.value = u.city;
  newState.value = u.state;
  newCountry.value = u.country;
  newDiagnosis.value = u.diagnosis;
}

function updateUser() {
  console.log("click");
  Axios.put(BASE_URL + id.value, {
    id: id.value,
    fullName: newFullName.value,
    age: newAge.value,
    city: newCity.value,
    state: newState.value,
    country: newCountry.value,
    diagnosis: newDiagnosis.value,
  }).then((res) => console.log(res));
  // window.location.reload();
}
function deleteUser(id) {
  Swal.fire({
    title: "Are you sure?",
    text: "You won't be able to revert this!",
    icon: "warning",
    showCancelButton: true,
    confirmButtonColor: "#3085d6",
    cancelButtonColor: "#d33",
    confirmButtonText: "Yes, delete it!",
  }).then((result) => {
    if (result.isConfirmed) {
      deleteFunction(id);

      Swal.fire("Deleted!", "Your file has been deleted.", "success");
      loadUsers();
    }
  });
  console.log(id, "id of user");
}

function deleteFunction(id) {
  Axios.delete(BASE_URL + id).then((res) => console.log(res));
}
</script>

<style></style>
