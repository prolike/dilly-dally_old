<template>
  <AppHeaderDropdown right no-caret>
    <template slot="header">
      <img :src="getImageUrl()" class="img-avatar" />
    </template>\
    <template slot="dropdown">
      <b-dropdown-header tag="div" class="text-center"><strong>Logged in as:<br>{{ getUserName()}}</strong></b-dropdown-header>
      <b-dropdown-item><i class="fa fa-user" /> Profile</b-dropdown-item>
      <b-dropdown-item><i class="fa fa-wrench" /> Settings</b-dropdown-item>
      <b-dropdown-item v-on:click="logOut()"><i class="fa fa-lock" /> Logout</b-dropdown-item>
    </template>
  </AppHeaderDropdown>
</template>
<script>
import { HeaderDropdown as AppHeaderDropdown } from '@coreui/vue'
import { firebaseHandler } from '../controller/firebaseHandler';

export default {
  name: 'DefaultHeaderDropdownAccnt',
  components: {
    AppHeaderDropdown
  },
  data: function() {
    return {
      user: ""
    }
  },
  mounted() {
    this.checkUser()
  },
  methods: {
    checkUser() {
      if (firebaseHandler.isReady() == false) {
        console.log("IS NOT SIGNED IN")
        window.setTimeout(this.checkUser, 100); /* this checks the flag every 100 milliseconds*/
      } else {
        console.log("IS SIGNED IN")
        this.user = firebaseHandler.getUser()
        console.log(this.user)
      }
    },
    getImageUrl() {
      return this.user.photoURL;
    },
    getUserName() {
      return this.user.displayName;
    },
    getEmail() {
      return this.user.email;
    },
    logOut() {
      console.log("logging out")
      firebaseHandler.logOut()
    },
  },
}

</script>
