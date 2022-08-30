<template>
    <div v-if="!loading" class="flex justify-center items-center">
        <!-- <h1>Profile</h1> -->
        <div v-if="fbData !== null" class="flex flex-col items-center mt-24">
            <p class="uppercase bg-blue-200 rounded-lg w-20 h-20 flex 
            justify-center items-center font-bold text-2xl">
                {{fbData['first name'][0]}}{{fbData['last name'][0]}}
            </p>
            <div class="flex flex-col gap-2 my-4 text-lg">
                <p class="flex justify-between">Email: <span class="ml-1 hover:cursor-not-allowed">{{fbData.email}}</span></p>
                <div class="flex justify-between">First Name:<input type="text" v-model="fbData['first name']" class="border px-2 ml-1">
                </div>
                <div class="flex justify-between">Last Name: <input type="text" v-model="fbData['last name']" class="border px-2 ml-1"></div>
                <div class="flex justify-between">Age:<input type="text" v-model="fbData.age" class="border px-2 ml-1"></div>
            </div>
            <button @click="handleSave" class="border py-3 px-7 rounded-full shadow 
            font-semibold bg-sky-400 text-white mt-4">Save Changes</button>
        </div>
        <div v-else>
            <p>No user is logged in</p>
        </div>
    </div>
    <div v-else>
        Loading...
    </div>
</template>
<script>
import { ref, onMounted, onUnmounted } from 'vue'
import { doc, setDoc, getDoc, updateDoc } from "firebase/firestore"; 
import {db, auth} from '../firebase/config'
import { getAuth, onAuthStateChanged } from "firebase/auth";

import { useRouter } from 'vue-router'

export default{
    setup() {
        const fbData = ref(null);
        const loading = ref(true);

        const router = useRouter();

        onMounted(()=>{
            const unsub = onAuthStateChanged(auth, async(user) => {
                if(user){
                    const docSnap = await getDoc(doc(db, "users", user.uid));
        
                    fbData.value = {...docSnap.data(), id: docSnap.id}
                } else{
                    console.log("user is not logged in")
                }
                unsub();
                loading.value = false;
            })
            
            // const docSnap = await getDoc(doc(db, "users", "QV4P6dSx6zdEAqv3Y7Jcuzqey4x2"));
        
            // fbData.value = {...docSnap.data(), id: docSnap.id}
        })

        const handleSave = async () =>{
            await updateDoc(doc(db, "users", fbData.value.id), {
                age: fbData.value.age,
                'first name': fbData.value['first name'],
                'last name': fbData.value['last name'],
            })
            router.push('/')
        }

        return {fbData, loading, handleSave}
    }
}
</script>

<style>
[v-cloak] {display: none}
</style>