<template>
<div class="landingpage">
    <v-layout row wrap>
          <v-flex xs6 sm8 md9>
            <v-container text-xs-center black--text>
              <h1> StockTrading</h1>
              <H2>"Let it watch the market for you while you play"</H2><br>
          </v-container>
          </v-flex>
          <v-flex xs6 sm4 md3 >
            <v-card class="ma-2">
              <v-tabs grow light>
                <v-tab>Sign In</v-tab>
                <v-tab>Register</v-tab>
                <v-tab-item>
                  <v-form ref="form" v-model="valid" lazy-validation>
                    <v-card-text>
                      <v-form>
                        <v-alert :value="error.msg" type="error">
                          {{error.msg}}
                        </v-alert>
                        <v-text-field
                          prepend-icon="person"
                          v-model="email"
                          label="Email"
                          type="text"
                          :rules="emailRule">
                        </v-text-field>
                        <v-text-field
                          prepend-icon="lock"
                          v-model="password"
                          label="Password"
                          type="password"
                          :counter="6"
                          :rules="passwordRule">
                        </v-text-field>
                      </v-form>
                    </v-card-text>
                    <v-card-actions>
                      <v-btn color="blue" light @click="signIn()" :disabled="!valid" left>Login</v-btn>
                    </v-card-actions>
                  </v-form>
                </v-tab-item>
                <v-tab-item>
                  <v-card>
                    <v-card-text>
                      <v-container>
                        <span style="red--text"> Registration is not open to the public as this time. </span>
                        <!-- <sign-up/> -->
                      </v-container>
                    </v-card-text>
                  </v-card>
                </v-tab-item>
              </v-tabs>
            </v-card>
          </v-flex>
        </v-layout>
  </div>
</template>
<script>
import SignUp from './Signup'

export default {
  name: 'LandingPage',
  components: {
    'sign-up': SignUp
  },
  data () {
    return {
      valid: true,
      error: { msg: '' },
      email: '',
      emailRule: [
        v => !!v || 'Email is required',
        v => /.+@.+/.test(v) || 'E-mail must be valid'
      ],
      password: '',
      passwordRule: [
        v => !!v || 'Password is required',
        v => (v && v.length >= 6) || 'Password must be more than 6 characters'
      ]
    }
  },
  methods: {
    signIn () {
      if (this.valid) {
        this.$store.dispatch('setLoadin', true)
        this.$store.dispatch('signIn', {'email': this.email, 'password': this.password})
        .then(user => {
          console.log('Login the user')
        }, error => {
          this.$store.dispatch('setLoadin', false)
          this.error.msg = error.message
          setTimeout(() => {
            this.error.msg = ''
          }, 2000)
        })
      }
    },
    signUp () {
      if (this.valid) {
        this.$store.dispatch('setLoadin', true)
        this.$store.dispatch('signUp', {
          firstName: this.firstName,
          lastName: this.lastName,
          email: this.email,
          password: this.password})
        .then(user => {
          console.log('Signup the user')
        }, error => {
          this.$store.dispatch('setLoadin', false)
          this.error.msg = error.message
          var that = this
          setTimeout(function () {
            that.error.msg = ''
          }, 2000)
        })
      }
    }
  }
}
</script>

<style>
  .landingpage {
    background-color: white;
    background-size: cover;
    background-image: url("../assets/bg-image.jpeg");
    height: 100%;
    min-height: 500px;
    border: blue solid 3px;
    padding: 5px;
    border-radius: 10px;
  }
  .round {
    border-radius: 10px;
  }
</style>
