<template>
  <div>
    <h3 v-if="loggedIn">Welcome! Please Select An Option To Begin!</h3>
    <b-form v-else @submit="onSubmit" >
      <b-form-group
        id="input-group-1"
        label="Email address:"
        label-for="input-1"
        description="We'll never share your email with anyone else."
      >
      <b-form-input
        id="input-1"
        type="email"
        required
        placeholder="Enter email"
        v-model="form.email"
      >
      </b-form-input>
      </b-form-group>

      <b-form-group
        id="input-group-2"
        label="Password:"
        label-for="input-2"
      >
      <b-form-input
        id="input-2"
        type="password"
        required
        placeholder="Enter password"
        v-model="form.password"
      >
      </b-form-input>
      </b-form-group>

      <b-button type="submit" variant="primary">Submit</b-button>
    </b-form>
    <br><br>
  <div>
    <b-card-group deck>
      <b-card bg-variant="info" text-variant="white" header="Courses" class="text-center">
        <b-button variant="light"> <router-link :to="`/courses/`">View All Courses Currently Available</router-link></b-button>
      </b-card>

      <b-card bg-variant="info" text-variant="white" header="Lecturers" class="text-center">
        <b-button variant="light"> <router-link :to="`/lecturers/`">View All Lecturers Currently Teaching</router-link></b-button>
      </b-card>

      <b-card bg-variant="info" text-variant="white" header="Enrolments" class="text-center">
        <b-button variant="light"> <router-link :to="`/enrolments/`">View All College Enrolments</router-link></b-button>
      </b-card>
    </b-card-group>
  </div>

  <br><br><br>
  <b-button variant="outline-info" @click="logout">Log Out</b-button>

</div>

</template>
<script>
export default {
  name: 'index',
  components: {

  },
  data() {
    return {
      form: {
        email: '',
        password: ''
      },
      loggedIn: false
    }
  },
  created() {
    if (localStorage.getItem('token')) {
      this.loggedIn = true;
    }
    else {
      this.loggedIn = false;
    }
  },
  methods: {

    onSubmit(evt) {
      evt.preventDefault();

      let app = this;

      axios.post('/api/login', {
        email: app.form.email,
        password: app.form.password
      })
      .then(function(response) {
        //Once logged in, the session token is stored in local data for authorisation throughout the application
        localStorage.setItem('token', response.data.token)
        app.loggedIn = true;
      })
      .catch(function(error) {
        console.log(error.response.data);
      });
    },
    logout() {
      localStorage.removeItem('token');
      console.log("Logged Out");
    }
  }
}
</script>
<style>
</style>
