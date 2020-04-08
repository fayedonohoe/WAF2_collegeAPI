<template>
  <b-row align-h="center">
    <b-col cols="12">

      <b-table-simple hover responsive bordered>
        <b-thead>
          <b-tr>
            <b-th>Title</b-th>
            <b-th>Code</b-th>
            <b-th>Description</b-th>
            <b-th>Points</b-th>
            <b-th>Level</b-th>
            <b-th>Actions</b-th>
          </b-tr>
        </b-thead>
        <b-tbody>
          <b-tr v-for="item in items" :key="item.id">
            <b-td>{{ item.title }}</b-td>
            <b-td>{{ item.code }}</b-td>
            <b-td>{{ item.description }}</b-td>
            <b-td>{{ item.level }}</b-td>
            <b-td>{{ item.points }}</b-td>
            <b-td>
              <b-button variant="outline-primary"> <router-link :to="`/courses/show/${item.id}`">View</router-link></b-button>
              <b-button variant="outline-primary"> <router-link :to="`/courses/edit/${item.id}`">Edit</router-link></b-button>
              <b-button variant="outline-primary" @click="deleteCourse(item.id)"> Delete </b-button>
            </b-td>
          </b-tr>
        </b-tbody>
      </b-table-simple>
    </b-col>
  </b-row>
</template>
<script>
export default {
  data() {
    return {
      items: []   // stores result of axios get request
    }
  },
  created(){
    let app = this;
    let token = localStorage.getItem('token');
    axios.get('/api/courses', {
      headers: { Authorization: "Bearer " + token}
    })
    .then(function (response) {
       console.log(response.data);
       app.items = response.data.data; // how the data is stored
    })
    .catch(function (error) {
       console.log(error);
    })
  },
  methods: {

    deleteCourse(id) {
      let app = this;
      let token = localStorage.getItem('token');
      axios.delete('/api/courses/' + id, {
        headers: { Authorization: "Bearer " + token}
      })
      .then(function (response) {
         console.log(response.data);
         app.items = app.items.filter(dat => dat.id !== id); // returns everything but the id you just deleted by accessing the DOM
      })
      .catch(function (error) {
         console.log(error);
      })
    }
  }
}
</script>
<style>
</style>
