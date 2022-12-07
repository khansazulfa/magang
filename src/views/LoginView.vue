<template>
  <div class="pa-10">
    <v-form
        ref="formLogin"
        lazy-validation
      >
        <v-text-field
          v-model="form.email"
          label="Name"
          required
        ></v-text-field>

        <v-text-field
          v-model="form.password"
          label="Password"
          type="password"
          required
        ></v-text-field>

        <v-btn
          color="success"
          class="mr-4"
          @click="login"
        >
          Login
        </v-btn>
      </v-form>

      <div class="alert--positon">
        <v-alert v-if="alert.error" type="error">
          {{ error.message }}
        </v-alert>

        <v-alert v-if="alert.success" type="success">
          {{ error.message }}
        </v-alert>
      </div>
  </div>
</template>

<script>
  export default {
    data: () => ({
      form: {
        email: '',
        password: ''
      },
      alert: {
        error: false,
        success: false,
      },
      error: {
        message: '',
      }
    }),
    methods: {
      login () {
        if(this.$refs.formLogin.validate()) {
          this.axios.post("http://13.214.194.163:5400/api/v1/auth/login", this.form)
          .then(response => {
              localStorage.setItem('user', JSON.stringify(response.data.data));
              this.alert.success = true;
              setTimeout(() => {
                this.alert.success = false;
                this.$router.push('/');
              }, 3000);
            })
          .catch(err => {
            this.alert.error = true;
            setTimeout(() => {
              this.alert.error = false;
            }, 3000);
            this.error.message = err.response.data.message;
          })
        }
       },
      // reset () {
      //   this.$refs.form.reset()
      // },
      // resetValidation () {
      //   this.$refs.form.resetValidation()
      // },
    },
  }
</script>

<style lang="scss">
.alert--positon {
  position: fixed;
  top: 20px;
  right: 20px;
}
</style>