<template lang="html">
    <nav class="navbar container-fluid">

      <div class="row">
          <div class="nav-logo left">
            <router-link :to="{ name: 'Index' }">
              <a href="#"></a>
              <img class="logo" src="@/assets/S-PLD_logo.png">
            </router-link>
          </div>

          <div class="nav-content hide-on-med-and-down container-fluid">
            <ul class="right">
              <li v-if="!user"> <router-link :to="{ name: 'Signup' }">Signup</router-link></li>
              <li v-if="!user"> <router-link :to="{ name: 'Login'}">Login</router-link></li>

              <!-- emulating a button -->

              <li v-if="user"> <a href="#">{{ user.email }}</a> </li>
              <li v-if="user"> <a @click="logout" href="#">Logout</a> </li>
            </ul>
          </div>

          <div class="nav-content container-fluid">
            <ul class="right">
              <a data-target="slide-out" class="sidenav-trigger"><i class="material-icons">menu</i></a>
            </ul>
          </div>


            <ul id="slide-out" class="nav-content sidenav container">
              <li v-if="!user" @click="closeSideNav"> <router-link  :to="{ name: 'Signup' }">Signup</router-link></li>
              <li v-if="!user" @click="closeSideNav"> <router-link :to="{ name: 'Login'}">Login</router-link></li>
              <li v-if="user"> <a href="#">{{ user.email }}</a> </li>
              <li v-if="user"> <a @click="logout" href="#">Logout</a> </li>
            </ul>





          <div class="nav-bottom container-fluid">
            <div class="center">
              <router-link :to="{ name: 'Index' }" class="">
                Community Playlists
              </router-link>
              <!-- <div class="field right">
                <input type="text" class="col s4">
              </div> -->
            </div>

          </div>

      </div>

    </nav>
</template>

<script>
import database from '@/firebase/init'
import firebase from 'firebase'
import 'firebase/auth' // production/dev differences
export default {
  name: 'Navbar',
  data(){
    return{
      user: null,
      alias: null,
      instance: null
    }
  },
  methods: {
    logout(){
      //asynchronous method, returns a promise
      firebase.auth().signOut().then( () => {
        //redirect
        console.log();
        this.$router.push('Login');
      })
    },
    closeSideNav(){
      document.getElementById('slide-out').click();
    }
  },

  created(){

    // let user = firebase.auth().currentUser
    // runs for every status change
    firebase.auth().onAuthStateChanged((user) => {
      console.log(user)
      if(user){
        this.user = user
        // let ref = database.collection('users').doc('andy')
        // ref.get().then(doc => {
        //   console.log(doc)
        //   this.alias = doc.alias
        // })
      } else {
        this.user = null
      }
    })
  },
  mounted(){
    let elems = this.$el.querySelectorAll('.sidenav');
    this.instances = M.Sidenav.init(elems, {
      edge: 'right',
      closeOnClick: true,
      draggable: true
    });
    this.instance = M.Sidenav.getInstance(elems)
  }

}

</script>

<style lang="css">
 nav{
  height: 100px;
  padding: 0 20px;
  background-color: #0F121D;
  font-family: 'Revalia', regular;

}

nav .nav-bottom{
  display: block;
  position: absolute;
  margin-left: auto;
  margin-right: auto;
  top: 45px;
  left: 0;
  right: 0;
}

nav .sidenav-trigger{
  cursor: pointer;
}


.nav-logo .logo{
  padding-top: 10px;
  width:180px; /* you can use % */
  height: auto;
}




</style>
