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
            v-model="form.date"
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
            v-model="form.time"
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
          <b-form-input
            id="input-3"
            type="text"
            required
            placeholder="Enter Status"
            v-model="form.status"
          >
          </b-form-input>
          </b-form-group>

          <b-form-group
            id="input-group-4"
            label="Course:"
            label-for="input-4"
          >
          <b-form-input
            id="input-4"
            type="number"
            required
            placeholder="Enter Course"
            v-model="form.course_id"
          >
          </b-form-input>
          </b-form-group>

          <b-form-group
            id="input-group-5"
            label="Lecturer:"
            label-for="input-5"
          >
          <b-form-input
            id="input-5"
            type="number"
            required
            placeholder="Enter Lecturer"
            v-model="form.lecturer_id"
          >
          </b-form-input>
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
      form: {
        date: '',
        time: '',
        status: '',
        course: '',
        lecturer: ''
      },
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
  },
  methods: {
    onSubmit(evt) {
      evt.preventDefault()

      let app = this;
      let token = localStorage.getItem('token');

      axios.post('/api/enrolments', {
        date: app.form.date,
        time: app.form.time,
        status: app.form.status,
        course_id: app.form.course_id,
        lecturer_id: app.form.lecturer_id
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
