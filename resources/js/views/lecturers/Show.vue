<template>
  <b-row align-h="center">
    <b-col cols="12">

      <b-button variant="outline-primary"> <router-link :to="`/lecturers/`">Back</router-link> </b-button>

      <h2> Lecturer Information </h2>

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
            <b-td>{{ lecturer.name }}</b-td>
            <b-td>{{ lecturer.address }}</b-td>
            <b-td>{{ lecturer.phone }}</b-td>
            <b-td>{{ lecturer.email }}</b-td>
            <b-td>

              <b-button variant="outline-primary"> <router-link :to="`/lecturer/edit/${lecturer.id}`">Edit</router-link></b-button>
              <b-button variant="outline-danger" @click="deleteLecturer(lecturer.id)"> Delete </b-button>

            </b-td>
          </b-tr>
        </b-tbody>
      </b-table-simple>
    </b-col>
    <b-col cols="12">
        <h3> Courses Enrolled In </h3>
      <b-table-simple hover responsive bordered>
        <b-thead>
          <b-tr>
            <b-th>Date</b-th>
            <b-th>Time</b-th>
            <b-th>Status</b-th>
            <b-th>Course Name</b-th>


          </b-tr>
        </b-thead>
        <b-tbody>
            <b-tr v-for="enrolments in lecturer.enrolments" :key="enrolments.id">
              <b-td>{{ enrolments.date }}</b-td>
              <b-td>{{ enrolments.time }}</b-td>
              <b-td>{{ enrolments.status }}</b-td>
              <b-td>{{ enrolments.course.title }}</b-td>
              </b-tr>
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
      lecturer: {},
      show: true
    }
  },
    created() {
      // console.log(localStorage.getItem('token'));
      if (localStorage.getItem('token')) {
        this.loggedIn = true;
      }
      else {
        this.loggedIn = false;
      }

      let app = this;
      let token = localStorage.getItem('token');
      axios.get(`/api/lecturers/${app.$route.params.id}`, {
        headers: { Authorization: "Bearer " + token }
      })
      .then(function (response) {
        app.lecturer = response.data.data;
      })
      .catch(function (error) {
        console.log(error);
      });

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
