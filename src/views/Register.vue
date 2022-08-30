<template>
    <div class="mx-6">
        <div>
            <h1 class="font-bold text-2xl my-4">Register</h1>
            <form @submit.prevent="handleClick" class="flex flex-col gap-3">
                <!-- <input type="text" placeholder="first name" class="py-1 px-3 w-full border rounded">
                <input type="text" placeholder="last name" class="py-1 px-3 w-full border rounded">
                <input type="number" placeholder="age" min="0" class="py-1 px-3 w-full border rounded">
                <input type="email" placeholder="email" class="py-1 px-3 w-full border rounded">
                <input type="password" placeholder="password" class="py-1 px-3 w-full border rounded"> -->
                <div class="relative">
                    <input v-model="firstName" type="text" id="firstName" class="block rounded-t-lg px-2.5 pb-2.5 pt-5 w-full text-sm text-gray-900 bg-gray-50 dark:bg-gray-700 border-0 border-b-2 border-gray-300 appearance-none dark:text-white dark:border-gray-600 dark:focus:border-blue-500 focus:outline-none focus:ring-0 focus:border-blue-600 peer" placeholder=" " />
                    <label for="firstName" class="absolute text-sm text-gray-500 dark:text-gray-400 duration-300 transform -translate-y-4 scale-75 top-4 z-10 origin-[0] left-2.5 peer-focus:text-blue-600 peer-focus:dark:text-blue-500 peer-placeholder-shown:scale-100 peer-placeholder-shown:translate-y-0 peer-focus:scale-75 peer-focus:-translate-y-4">First name</label>
                </div>
                <div class="relative">
                    <input v-model="lastName" type="text" id="lastName" class="block rounded-t-lg px-2.5 pb-2.5 pt-5 w-full text-sm text-gray-900 bg-gray-50 dark:bg-gray-700 border-0 border-b-2 border-gray-300 appearance-none dark:text-white dark:border-gray-600 dark:focus:border-blue-500 focus:outline-none focus:ring-0 focus:border-blue-600 peer" placeholder=" " />
                    <label for="lastName" class="absolute text-sm text-gray-500 dark:text-gray-400 duration-300 transform -translate-y-4 scale-75 top-4 z-10 origin-[0] left-2.5 peer-focus:text-blue-600 peer-focus:dark:text-blue-500 peer-placeholder-shown:scale-100 peer-placeholder-shown:translate-y-0 peer-focus:scale-75 peer-focus:-translate-y-4">Last name</label>
                </div>
                <div class="relative">
                    <input v-model="age" type="number" min="0" id="age" class="block rounded-t-lg px-2.5 pb-2.5 pt-5 w-full text-sm text-gray-900 bg-gray-50 dark:bg-gray-700 border-0 border-b-2 border-gray-300 appearance-none dark:text-white dark:border-gray-600 dark:focus:border-blue-500 focus:outline-none focus:ring-0 focus:border-blue-600 peer" placeholder=" " />
                    <label for="age" class="absolute text-sm text-gray-500 dark:text-gray-400 duration-300 transform -translate-y-4 scale-75 top-4 z-10 origin-[0] left-2.5 peer-focus:text-blue-600 peer-focus:dark:text-blue-500 peer-placeholder-shown:scale-100 peer-placeholder-shown:translate-y-0 peer-focus:scale-75 peer-focus:-translate-y-4">Age</label>
                </div>
                <div class="relative">
                    <input v-model="email" type="email" id="email" class="block rounded-t-lg px-2.5 pb-2.5 pt-5 w-full text-sm text-gray-900 bg-gray-50 dark:bg-gray-700 border-0 border-b-2 border-gray-300 appearance-none dark:text-white dark:border-gray-600 dark:focus:border-blue-500 focus:outline-none focus:ring-0 focus:border-blue-600 peer" placeholder=" " />
                    <label for="email" class="absolute text-sm text-gray-500 dark:text-gray-400 duration-300 transform -translate-y-4 scale-75 top-4 z-10 origin-[0] left-2.5 peer-focus:text-blue-600 peer-focus:dark:text-blue-500 peer-placeholder-shown:scale-100 peer-placeholder-shown:translate-y-0 peer-focus:scale-75 peer-focus:-translate-y-4">Email</label>
                </div>
                <div class="relative">
                    <input v-model="password" type="password" id="password" class="block rounded-t-lg px-2.5 pb-2.5 pt-5 w-full text-sm text-gray-900 bg-gray-50 dark:bg-gray-700 border-0 border-b-2 border-gray-300 appearance-none dark:text-white dark:border-gray-600 dark:focus:border-blue-500 focus:outline-none focus:ring-0 focus:border-blue-600 peer" placeholder=" " />
                    <label for="password" class="absolute text-sm text-gray-500 dark:text-gray-400 duration-300 transform -translate-y-4 scale-75 top-4 z-10 origin-[0] left-2.5 peer-focus:text-blue-600 peer-focus:dark:text-blue-500 peer-placeholder-shown:scale-100 peer-placeholder-shown:translate-y-0 peer-focus:scale-75 peer-focus:-translate-y-4">Password</label>
                </div>
                <button class="p-3 bg-emerald-300">Register</button>
            </form>
        </div>
        <div>
            <router-link to="/login" class="underline">Already have an account?</router-link>
        </div>    
    </div>
    
</template>
<script>
import {createUserWithEmailAndPassword} from "firebase/auth";
import { ref } from 'vue'
import { doc, setDoc, } from "firebase/firestore"; 
import {db, auth} from '../firebase/config'

import { useRouter } from 'vue-router'

export default {
    setup() {
        const firstName = ref(null);
        const lastName = ref(null);
        const age = ref(null);
        const email = ref(null);
        const password = ref(null);

        const router = useRouter();

        const handleClick = () =>{
            createUserWithEmailAndPassword(auth, email.value, password.value)
                .then((userCredential)=>{
                    setDoc(doc(db, "users", userCredential.user.uid), {
                        "first name": firstName.value,
                        "last name": lastName.value,
                        "email": email.value,
                        "age": age.value
                    })
                    email.value = password.value = firstName.value = lastName.value = age.value = ""

                    //set an empty doc for drive
                    setDoc(doc(db, "textareaUser", userCredential.user.uid), {
                        userTxtDoc: []
                    })

                    router.push('/')
                })
                .catch((err)=>{
                    console.log(err)
                })
        }

        return {handleClick, email, firstName, lastName, age, password}
    }
}
</script>