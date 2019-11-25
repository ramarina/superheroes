<template>
  <div>
    <b-navbar toggleable="lg" type="dark" variant="dark">
      <b-navbar-brand href="#">Superheroes</b-navbar-brand>
      <b-navbar-toggle target="nav_collapse"></b-navbar-toggle>
      <b-collapse is-nav id="nav_collapse">
        <b-navbar-nav class="ml-auto">
          <b-nav-item-dropdown right>
            <template slot="button-content">
              <em>{{ (authenticatedUser) ? authenticatedUser.email : 'Guest' }}</em>
            </template>
            <b-dropdown-item v-if="!authenticatedUser" href="/login">Login</b-dropdown-item>
            <b-dropdown-item v-if="authenticatedUser" @click="logout">Logout</b-dropdown-item>
          </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>

<script>
import firebase from 'firebase';

export default {
    name: "Navbar",
    props: [
        'authenticatedUser',
    ],
    methods: {
        logout: function() {
            firebase.auth().signOut().then(() => {
                this.$router.replace('login')
            })
        }
    }
}
</script>
