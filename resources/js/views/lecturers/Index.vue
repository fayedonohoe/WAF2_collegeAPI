<template>
  <b-row align-h="center">
    <b-col cols="12">

      <b-table-simple hover responsive bordered>
        <b-thead>
          <b-tr>
            <b-th>Name</b-th>
            <b-th>Address</b-th>
            <b-th>Phone Number</b-th>
            <b-th>Email</b-th>
            <b-th>Actions</b-th>
          </b-tr>
        </b-thead>
        <b-tbody>
          <b-tr v-for="item in items" :key="item.id">
            <b-td>{{ item.name }}</b-td>
            <b-td>{{ item.address }}</b-td>
            <b-td>{{ item.phone }}</b-td>
            <b-td>{{ item.email }}</b-td>
            <b-td>
              <b-button variant="outline-primary"> <router-link :to="`/lecturers/show/${item.id}`">View</router-link></b-button>
              <b-button variant="outline-primary"> <router-link :to="`/lecturers/edit/${item.id}`">Edit</router-link></b-button>
              <b-button variant="outline-primary" @click="deleteLecturer(item.id)">Delete</b-button>
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
      items: []
    }
  },
  created(){
    let app = this;
    let token = localStorage.getItem('token');
    axios.get('/api/lecturers', {
      headers: { Authorization: "Bearer " + token}
    })
    .then(function (response) {
       console.log(response.data);
       app.items = response.data.data;
    })
    .catch(function (error) {
       console.log(error);
    })
  },

  methods: {
    deleteLecturer(id) {
      let app = this;
      let token = localStorage.getItem('token');
      axios.delete('/api/lecturers/' + id, {
        headers: { Authorization: "Bearer " + token}
      })
      .then(function (response) {
         console.log(response.data);
         app.items = app.items.filter(dat => dat.id !== id);
      })
      .catch(function (error) {
         console.log(error);
      });
    }
  }
}
</script>
<style>
</style>
