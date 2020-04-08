<template>
  <b-row>
    <b-col cols="8">

      <b-card title="Add Enrolment" tag="article">
        <b-form @submit="onSubmit">
          <b-form-group
            id="input-group-1"
            label="Date:"
            label-for="input-1"
          >
          <b-form-input
            id="input-1"
            type="date"
            required
            placeholder="Enter Date"
            v-model="enrolment.date"
          >
          </b-form-input>
          </b-form-group>

          <b-form-group
            id="input-group-2"
            label="Time:"
            label-for="input-2"
          >
          <b-form-input
            id="input-2"
            type="time"
            required
            placeholder="Enter Time"
            v-model="enrolment.time"
          >
          </b-form-input>
          <!-- <b-form-invalid-feedback :state="codeValid">
           The code may not be greater than 5 characters.
          </b-form-invalid-feedback>
          <b-form-valid-feedback :state="codeValid">
            Looks Good.
          </b-form-valid-feedback> -->
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
              v-model='enrolment.course'
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
              v-model='enrolment.lecturer'
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
      enrolment:{},
        status: null,
        courses: '',
        lecturers: '',
        status: [{ text : 'Select One', value:null}, 'assigned', 'associate', 'career_break','interested'],

      loggedIn: false,
      errors: []
    }
  },
  // computed: {
  //   codeValid() {
  //     return this.form.code.length <= 5 && this.form.code.length > 0
  //   }
  // },
  created() {
    if (localStorage.getItem('token')) {
      this.loggedIn = true;
    }
    else {
      this.loggedIn = false;
      this.$router.push('/');
    }
    let app =this;
    let token = localStorage.getItem('token');
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

      let app = this;
      let token = localStorage.getItem('token');

      axios.post('/api/enrolments', {
        date: app.enrolment.date,
        time: app.enrolment.time,
        status: app.enrolment.status,
        course_id: app.enrolment.course,
        lecturer_id: app.enrolment.lecturer
      }, {
        headers: { Authorization: `Bearer ${token}`}
      })
      .then(function(response) {
        app.$router.push('/enrolments');
      })
      .catch(function (error) {
        console.log(error.response.data);

        app.errors = error.response.data.errors
      });


    }
  }
}
</script>
