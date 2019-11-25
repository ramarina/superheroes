<template>
  <div>
    <navbar :authenticatedUser="authenticatedUser"/> <br>
    <superheroes v-if="authenticatedUser"/>
  </div>
</template>

<script>
import firebase from 'firebase';
import Superheroes from '~/components/Superheroes.vue'
import Navbar from '~/components/Navbar.vue'

export default {
    components: {
        Superheroes,
        Navbar
    },
    data() {
        return {
            authenticatedUser: null
        }
    },
    created() {
        firebase.auth().onAuthStateChanged(user => (this.authenticatedUser = user))
    },
}
</script>
