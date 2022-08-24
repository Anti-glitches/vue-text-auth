<template>
    <div class="bg-green-300 text-center p-4 font-semibold underline text-blue-600 flex gap-2">
        <router-link to="/">Home</router-link>
        <router-link to="/profile">Profile</router-link>
        <router-link to="/dashboard">Dashboard</router-link>
        <div v-if="!loggedIn">
          <router-link to="/login">Login</router-link>
        </div>
        <div v-else-if="loggedIn">
          <p @click="handleClick" class="cursor-pointer">Logout</p>
        </div>
    </div>
</template>

<script>
import {onAuthStateChanged, signOut} from "firebase/auth";
import {auth} from '../firebase/config'
import {onMounted, ref} from 'vue'

export default{
  setup() {
    const loggedIn = ref(null)

    const handleClick = () =>{
      signOut(auth)
      .then(()=>{
        console.log('Logging you out', auth.currentUser);
        location.reload();
      }).catch((err)=>{
        console.log(err)
      })
    } 

    onMounted(()=>{
      onAuthStateChanged(auth, (user)=>{
        if(user){
          loggedIn.value = true;
        } else{
          loggedIn.value = false;
        }
      })
    })
    
    return {handleClick, auth, loggedIn}
  }
}


</script>