<template>
  <b-row align-h="center">
    <b-col cols="12">

      <b-button variant="outline-primary"> <router-link :to="`/enrolments/`">Back</router-link> </b-button>
      <br>

      <h2> Enrolment Information </h2>

      <b-table-simple hover responsive bordered>
        <b-thead>
          <b-tr>
            <b-th>Date</b-th>
            <b-th>Time</b-th>
            <b-th>Status</b-th>
            <b-th>Course</b-th>
            <b-th>Lecturer</b-th>
            <b-th>Actions</b-th>
          </b-tr>
        </b-thead>
        <b-tbody>
            <b-td>{{ enrolment.date }}</b-td>
            <b-td>{{ enrolment.time }}</b-td>
            <b-td>{{ enrolment.status }}</b-td>
            <b-td>{{ enrolment.course.title }}</b-td>
            <b-td>{{ enrolment.lecturer.name }}</b-td>
            <b-td>

              <b-button variant="outline-primary"> <router-link :to="`/enrolments/edit/${enrolment.id}`">Edit</router-link></b-button>
              <b-button variant="outline-danger" @click="deleteEnrolment(enrolment.id)"> Delete </b-button>

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
      enrolment: {},
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
      axios.get(`/api/enrolments/${app.$route.params.id}`, {
        headers: { Authorization: "Bearer " + token }
      })
      .then(function (response) {
        app.enrolment = response.data.data;
      })
      .catch(function (error) {
        console.log(error);
      });

    },
  methods: {
    deleteEnrolment(id) {
      let app = this;
      let token = localStorage.getItem('token');
      axios.delete('/api/enrolments/' + id, {
        headers: { Authorization: "Bearer " + token}
      })
      .then(function (response) {
         console.log(response.data);
         app.items = app.items.filter(dat => dat.id !== id);
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
