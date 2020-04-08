<template>
  <b-row align-h="center">
    <b-col cols="8">
      <h3 v-if="!loggedIn">You are not logged in!!</h3>
      <b-card
        v-else
        title="Edit Enrolment"
        tag="article"
      >

        <b-form @submit="onSubmit">
          <b-form-group
            id="input-group-1"
            label="Date:"
            label-for="input-1"
          >
            <b-form-input
              id="input-1"
              v-model="enrolment.date"
              type="date"
              required
              placeholder="Enter Date"
            ></b-form-input>
          </b-form-group>

          <b-form-group
            id="input-group-2"
            label="Time:"
            label-for="input-2"
          >
            <b-form-input
              id="input-2"
              v-model="enrolment.time"
              type="time"
              required
              placeholder="Enter Time"
            ></b-form-input>
          </b-form-group>

          <b-form-group
            id="input-group-3"
            label="Status:"
            label-for="input-3"
          >
            <b-form-select
              id="input-3"
              v-model="enrolment.status"
              :options="status"
              required

            ></b-form-select>
          </b-form-group>

          <b-form-group
            id="input-group-4"
            label="Course:"
            label-for="input-4"
          >
            <b-form-select
              id="input-4"
              name='courses'
              v-model='enrolment.course_id'
              class='form-control'>

              <option
              placeholder="Select a course"
              v-for="course in courses"
              :key= "course.id"
              v-bind:value="course.id">

                {{course.title}}

            </option>

            ></b-form-select>
          </b-form-group>
          <b-form-group
            id="input-group-5"
            label="Lecturers:"
            label-for="input-5"
          >
            <b-form-select
              id="input-5"
              name='lecturers'
              v-model='enrolment.lecturer_id'
              class='form-control'>

              <option
              placeholder="Select a Lecturer"
              v-for="lecturer in lecturers"
              :key= "lecturer.id"
              v-bind:value="lecturer.id">

                {{lecturer.name}}

            </option>

            ></b-form-select>
          </b-form-group>


          <b-button type="submit" variant="primary">Submit</b-button>
        </b-form>
      </b-card>
    </b-col>
  </b-row>
</template>

<script>
  export default {
    data() {
      return {
        enrolment: {},
        status: null,
        courses: '',  // store result of axios request into empty variable
        lecturers: '', // store result of axios request into empty variable
        show: true,
        loggedIn: false,
        status: [{ text : 'Select One', value:null}, 'assigned', 'associate', 'career_break','interested'],
        errors:[]
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
      //get all courses
      axios.get(`/api/courses`, {
        headers: { Authorization: "Bearer " + token }
      })
      .then(function (response) {
        app.courses = response.data.data;
      })
      .catch(function (error) {
        console.log(error);
      });
      // get all lecturers
      axios.get(`/api/lecturers`, {
        headers: { Authorization: "Bearer " + token }
      })
      .then(function (response) {
        app.lecturers = response.data.data;
      })
      .catch(function (error) {
        console.log(error);
      });

    },
    methods: {
      onSubmit(evt) {
        evt.preventDefault()

        // Stores the current information for this id in the models as placeholders
        let app = this;
        let token = localStorage.getItem('token');
        axios.put(`/api/enrolments/${app.$route.params.id}`, {
            date: app.enrolment.date,
            time: app.enrolment.time,
            status: app.enrolment.status,
            course_id: app.enrolment.course_id,
            lecturer_id: app.enrolment.lecturer_id
        },
        {
          headers: { Authorization: "Bearer " + token }
        })
        .then(function (response) {
          app.$router.push('/enrolments');
        })
        .catch(function (error) {
          console.log(error);
        });
      }
    }
  }
</script>
