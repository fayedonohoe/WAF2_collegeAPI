<template>
  <b-row align-h="center">
    <b-col cols="12">

      <b-button variant="outline-primary"> <router-link :to="`/courses/`">Back</router-link> </b-button>
      <br>

      <h2> Course Information </h2>

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
            <b-td>{{ course.title }}</b-td>
            <b-td>{{ course.code }}</b-td>
            <b-td>{{ course.description }}</b-td>
            <b-td>{{ course.level }}</b-td>
            <b-td>{{ course.points }}</b-td>
            <b-td>

              <b-button variant="outline-primary"> <router-link :to="`/courses/edit/${course.id}`">Edit</router-link></b-button>
              <b-button variant="outline-danger" @click="deleteCourse(course.id)"> Delete </b-button>

            </b-td>
          </b-tr>
        </b-tbody>
      </b-table-simple>
    </b-col>

    <b-col cols="12">
        <h3> Lecturer's Currently Enrolled in This Course </h3>
      <b-table-simple hover responsive bordered>
        <b-thead>
          <b-tr>
            <b-th>Date</b-th>
            <b-th>Time</b-th>
            <b-th>Status</b-th>
            <b-th>Lecturer Name</b-th>

          </b-tr>
        </b-thead>
        <b-tbody>
            <b-tr v-for="enrolments in course.enrolments" :key="enrolments.id">
            <b-td>{{ enrolments.date }}</b-td>
            <b-td>{{ enrolments.time }}</b-td>
            <b-td>{{ enrolments.status }}</b-td>
            <b-td>{{ enrolments.lecturer.name }}</b-td>

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
      course: {},
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
      axios.get(`/api/courses/${app.$route.params.id}`, {
        headers: { Authorization: "Bearer " + token }
      })
      .then(function (response) {
        app.course = response.data.data;
      })
      .catch(function (error) {
        console.log(error);
      });

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
