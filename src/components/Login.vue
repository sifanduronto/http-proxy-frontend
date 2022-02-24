<template>
  <v-container class="fill-height" fluid>
    <v-row align="center" justify="center">
      <v-col cols="12" sm="8" md="4" lg="3">
        <v-card class="elevation-5">
          <v-card-text class="py-5">
            <v-form v-on:keyup.enter="checkAuthentication">
              <v-text-field
                label="Login"
                name="login"
                prepend-icon="mdi-account"
                type="text"
                v-model="user.user"
              >
              </v-text-field>

              <v-text-field
                id="password"
                label="Password"
                name="password"
                prepend-icon="mdi-lock"
                :type="show ? 'text' : 'password'"
                v-on:keyup.enter="checkAuthintication"
                :append-icon="show ? 'mdi-eye' : 'mdi-eye-off'"
                v-model="user.pass"
                @click:append="show = !show"
              >
              </v-text-field>
            </v-form>
          </v-card-text>
          <v-card-actions class="pb-5">
            <v-btn
              :loading="loadingStatus"
              type="submit"
              color="primary"
              dark
              block
              @click="checkAuthentication"
              >Login</v-btn
            >
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
    <!-- SNACKBAR AND DIALOGS -->
    <v-snackbar
      v-model="snackbar"
      timeout="-1"
      :top="true"
      :right="true"
      :color="snackbarColor"
    >
      {{ snackbarText }}
      <template v-slot:action="{ attrs }">
        <v-btn text v-bind="attrs" @click="snackbar = false"> Close </v-btn>
      </template>
    </v-snackbar>
  </v-container>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Login',
  data () {
    return {
      user: {
        user: '',
        pass: ''
      },
      show: false,
      forgetPassword: false,
      loadingStatus: false,
      errorResponse: '',
      snackbar: false,
      snackbarText: '',
      snackbarColor: ''
    }
  },
  methods: {
     showSnackbar (status, text, color) {
      this.snackbar = status
      this.snackbarText = text
      this.snackbarColor = color
    },
    checkAuthentication () {
      axios.post('http://localhost:3000/login',this.user).then(result=>{
        this.showSnackbar(true,result.data.message,'success')
       
      }).catch(err=>{
        console.error(err)
        this.showSnackbar(true,'Proxy server could not be connected','error')
      })
    },
  }
}
</script>
