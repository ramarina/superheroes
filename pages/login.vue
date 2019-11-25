<template>
  <div class="container">
    <div class="login">
      <h3>Sign In</h3>
      <b-form-input type="text" v-model="email" placeholder="Email"></b-form-input><br>
      <b-form-input type="password" v-model="password" placeholder="Password"></b-form-input><br>
      <b-button @click="login">Login</b-button>
      <b-alert dismissible variant="danger" v-model="showDismissibleAlert">{{ error }}</b-alert>
      <p>
        or Sign In with Google <br>
        <b-button @click="socialLogin" class="social-button">
          <img alt="Google Logo" src="../assets/google-logo.png">
        </b-button>
      </p>
      <p>You don't have an account ? You can <nuxt-link to="/register">create one</nuxt-link></p>
    </div>
  </div>
</template>

<script>
import firebase from 'firebase';

export default {
    name: 'login',
    data() {
        return {
            email: '',
            password: '',
            showDismissibleAlert: false,
            error: null,
            authenticatedUser: null
        }
    },

    methods: {
        login() {
            firebase.auth().signInWithEmailAndPassword(this.email, this.password).then((user) => {
                this.$router.replace('home')
            }).catch((err) => {
                this.showDismissibleAlert = true
                this.error = err.message
            })
        },
        socialLogin() {
            const provider = new firebase.auth.GoogleAuthProvider();
            firebase.auth().signInWithPopup(provider).then((result) => {
                this.$router.replace('home')
            }).catch((err) => {
                this.error = err.message
            })
        },
    }
}
</script>
