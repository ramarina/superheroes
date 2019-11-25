<template>
  <div class="container">
    <div class="login">
      <h3>Register</h3>
      <b-form-input type="text" v-model="email" placeholder="Email"></b-form-input><br>
      <b-form-input type="password" v-model="password" placeholder="Password"></b-form-input><br>
      <b-button @click="signUp">Sign Up</b-button>
      <b-alert dismissible variant="danger" v-model="showDismissibleAlert">{{ error }}</b-alert>
      <p>or go back to <a href="#" @click="$emit('loging')">login</a></p>
    </div>
  </div>
</template>

<script>
import firebase from 'firebase';

export default {
    name: "Register",
    data() {
        return {
            email: '',
            password: '',
            error: null,
            showDismissibleAlert: false
        }
    },
    methods: {
        signUp: function() {
            firebase.auth().createUserWithEmailAndPassword(this.email, this.password).then(
                (user) => {
                    this.$emit('loging')
                },
                (err) => {
                    this.showDismissibleAlert = true
                    this.error = err.message
                }
            );
        }
    }
}
</script>

<style scoped>

</style>
