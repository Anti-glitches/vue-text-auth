<template>
    <div>
        dashboard
    </div>
    <div v-if="fbUserTxtDoc">
        <div class="flex gap-6 m-6">
            <div v-for="(doc, i) in fbUserTxtDoc" :key="i" @click="handleClick(doc)"
                class="w-60 h-44 bg-slate-200 rounded-lg cursor-pointer 
                flex flex-col justify-between shadow">
                <div class="flex justify-center items-center h-40">
                    <div class="bg-blue-400 w-20 h-20 rounded-xl flex justify-center items-center">
                        <i class="fa-solid fa-file-lines text-4xl text-white"></i>
                    </div>
                </div>
                <div class="flex gap-4 items-center bg-slate-100 p-3 px-4 w-60 rounded-b-lg">
                    <i class="fa-solid fa-file-lines text-2xl text-blue-400"></i>
                    <p class="text-base font-medium">{{doc.inputTitle}}</p>
                </div>
            </div>
        </div>

    </div>
</template>
<script>
import { onMounted, ref, watch } from 'vue';
import { doc, setDoc, getDoc, onSnapshot, updateDoc } from "firebase/firestore"; 
import {db, auth} from '../firebase/config'
import { getAuth, onAuthStateChanged } from "firebase/auth";

export default {
    setup() {
        const fbUserTxtDoc = ref(null);


        onMounted(async()=>{
            const docSnap = await getDoc(doc(db, 'textareaUser', 'OdSG7Kx0eopCIowYzWfh'));

            fbUserTxtDoc.value = docSnap.data().userTxtDoc
        })

        const handleClick = (doc) =>{
            console.log('hello', doc.textarea)
        }

        return {fbUserTxtDoc, handleClick}
    }
}
</script>