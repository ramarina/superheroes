<template>
  <div>
    <navbar :authenticatedUser="authenticatedUser"/>
    <br>
    <superheroes v-if="authenticatedUser"/>
    <login v-if="!authenticatedUser && !register" @registering="register = true"/>
    <register v-if="!authenticatedUser && register" @loging="register = false"/>
  </div>
</template>

<script>
import firebase from 'firebase';
import Superheroes from '~/components/Superheroes.vue'
import Navbar from '~/components/Navbar.vue'
import Login from  '~/components/Login'
import Register from  '~/components/Register'

export default {
    components: {
        Superheroes,
        Navbar,
        Login,
        Register
    },
    data() {
        return {
            authenticatedUser: null,
            register: false
        }
    },
    created() {
        firebase.auth().onAuthStateChanged(user => (this.authenticatedUser = user))
    },
}
</script>
