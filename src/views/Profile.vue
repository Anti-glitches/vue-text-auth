<template>
    <div v-if="!loading">
        <h1>Profile</h1>
        <div v-if="fbData !== null">
            <p class="uppercase bg-blue-200 rounded-full w-12 h-12 flex 
            justify-center items-center font-bold">
                {{fbData['first name'][0]}}{{fbData['last name'][0]}}
            </p>
            <p>Name: {{fbData['first name']}} {{fbData['last name']}}</p>
            <p>Email: {{fbData.email}}</p>
            <p>Age: {{fbData.age}}</p>
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
import { doc, setDoc, getDoc } from "firebase/firestore"; 
import {db, auth} from '../firebase/config'
import { getAuth, onAuthStateChanged } from "firebase/auth";

export default{
    setup() {
        const fbData = ref(null);
        const loading = ref(true);

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

        return {fbData, loading}
    }
}
</script>

<style>
[v-cloak] {display: none}
</style>