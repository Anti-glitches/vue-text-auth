<template lang="">
    <div>
        <h1 class="font-bold">Login</h1>
        <form @submit.prevent="handleClick">
            <input type="email" placeholder="email" v-model="email" class="w-full border">
            <input type="password" placeholder="password" v-model="password" class="w-full border">
            <button class="p-3 bg-emerald-300">Login</button>
        </form>
    </div>
    <div>
        <hr>
        <router-link to="/register" class="underline">Don't have an account?</router-link>
    </div>
</template>
<script>
import {createUserWithEmailAndPassword, signOut, signInWithEmailAndPassword } from "firebase/auth";
import { ref, onMounted } from 'vue'
import { doc, setDoc, getDoc } from "firebase/firestore"; 
import {db, auth} from '../firebase/config'

import { useRouter } from 'vue-router'

export default {
    setup() {
        const email = ref();
        const password = ref();

        const router = useRouter();

        const handleClick = () =>{
            signInWithEmailAndPassword(auth, email.value, password.value)
                .then((userCredential)=>{
                    console.log("you are logged in", userCredential)

                    getDoc(doc(db, "users", userCredential.user.uid))
                        .then((doc)=>{
                            console.log(doc.id, "=>", doc.data())

                            router.push('/profile')

                        }).catch(err=>{
                            console.log(err)
                        })

                    email.value = password.value = ""
                }).catch(err=>{
                    console.log(err);
                })
        }

        return {email, password, handleClick}
    }
}
</script>