<template>
    <div>
        dashboard
    </div>
    <div v-if="fbUserTxtDoc && selectedDoc === null">
        <div class="flex gap-6 m-6 bg-blue-100 cursor-pointer">
            <div v-for="(doc, i) in fbUserTxtDoc" :key="i" @click="handleClick(doc)"
                class="w-60 border flex gap-4 items-center bg-slate-100 p-3 px-4 rounded-lg shadow">
                    <i class="fa-solid fa-file-lines text-2xl text-blue-400"></i>
                    <p class="text-base font-medium">{{doc.inputTitle}}</p>
            </div>
        </div>
    </div>
    <div v-if="selectedDoc">
        {{selectedDoc}}
        <input type="text" v-model="selectedDoc.inputTitle">
        <button @click="handleSave" class="p-2 m-2 bg-green-300">Save Changes</button>
        <button @click="selectedDoc = null" class="p-2 m-2 bg-red-300">Go home</button>
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
        const selectedDoc = ref(null);

        onMounted(async()=>{
            const docSnap = await getDoc(doc(db, 'textareaUser', 'OdSG7Kx0eopCIowYzWfh'));

            fbUserTxtDoc.value = docSnap.data().userTxtDoc

            console.log(fbUserTxtDoc.value)
        })

        const handleClick = (doc) =>{
            selectedDoc.value = doc;
        }

        const handleSave = async() =>{
            await updateDoc(doc(db, "textareaUser", "OdSG7Kx0eopCIowYzWfh"), {
                userTxtDoc: fbUserTxtDoc.value
            })
        }

        return {fbUserTxtDoc, selectedDoc, handleClick, handleSave}
    }
}
</script>