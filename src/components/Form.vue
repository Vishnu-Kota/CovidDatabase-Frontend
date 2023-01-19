<template>
  <form @submit.prevent="createUser()" class="form-group container">
    <div
      class="container card shadow border p-0"
      style="width: 25rem; border-radius: 15px"
    >
      <h3
        class="card-header p-3 shadow-sm text-white bg-dark"
        style="border-top-left-radius: 15px; border-top-right-radius: 15px"
      >
        Register User
      </h3>
      <div>&;</div>
      <div class="card-body">
        <input
          v-model="FullName"
          placeholder="FullName"
          class="form-control mb-2"
        />
        <input
          v-model="Age"
          type="number"
          placeholder="Age"
          class="form-control mb-2"
        />
        <input v-model="City" placeholder="City" class="form-control mb-2" />
        <input v-model="State" placeholder="State" class="form-control mb-2" />
        <input
          v-model="Country"
          placeholder="Country"
          class="form-control mb-2"
        />

        <input
          v-model="Diagnosis"
          placeholder="Diagnosis"
          class="form-control mb-2"
        />
        <button class="btn btn-success my-2">Submit</button>
      </div>
    </div>
  </form>
</template>

<script setup>
import { ref } from "vue";
import Axios from "axios";
import Swal from "sweetalert2";

let Id = ref();
let FullName = ref("");
let Age = ref("");
let City = ref("");
let State = ref("");
let Country = ref("");
let Diagnosis = ref("");

const BASE_URL = "http://localhost:5282/Covid";

Axios.get(BASE_URL).then(function (res) {
  console.log(res);
  Id.value = res.data[0].Id;
  FullName.value = res.data[0].FullName;
  Age.value = res.data[0].Age;
  City.value = res.data[0].City;
  State.value = res.data[0].State;
  Country.value = res.data[0].Country;
  Diagnosis.value = res.data[0].Diagnosis;
});

function createUser() {
  Axios.post(BASE_URL, {
    age: Age.value,
    city: City.value,
    country: Country.value,
    fullName: FullName.value,
    state: State.value,
    Diagnosis: Diagnosis.value,
  }).then((res) => {
    Swal.fire("User has been added!", "", "success");
    window.location.reload();
    console.log(res);
  });
}
</script>

<style></style>
