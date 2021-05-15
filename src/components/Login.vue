<template>
  <div class="container">
    <div class="row">
      <form class="col-4 mx-auto bordered mt-5" @submit.prevent="login()">
          <h2>Login</h2>
          <div class="form-group">
            <label for="email">Email address</label>
            <input v-model="user.email" type="email" name="email" class="form-control" :class="{'is-invalid': errors.email}" id="email">
            <div v-if="errors.email" class="invalid-feedback">
              {{ errors.email[0] }}
            </div>
          </div>
          <div class="form-group">
            <label for="password">Password</label>
            <input v-model="user.password" type="password" name="password" class="form-control" id="password">
          </div>
          <button type="submit" class="btn btn-primary"><span v-if="loading" class="spinner-border"></span> Submit</button>
      </form>
    </div>
  </div>
</template>

<script>


import BaseRequest from '../core/BaseRequest';

export default {
  data() {
    return {
      user: {
        email: '',
        password: ''
      },
      errors: {},
      loading:false,

    }
  },
  methods: {
    login: function() {
      this.loading = true;
      BaseRequest.post('login', this.user)
      .then(response =>  {
        window.localStorage.setItem('token', response.data.token);
        this.$router.push({name: 'dashboard'});
      })
      .catch( error =>  {
        this.loading = false;
        this.errors = error.response.data.errors;
      });
    }
  }
}

</script>

<style>

</style>