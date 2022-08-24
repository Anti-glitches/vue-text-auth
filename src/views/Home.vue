<template>
    <div v-if="userData" class="bg-slate-100">
        <div class="bg-white border-b-2 px-24 py-8 mb-4 flex gap-3">
            <i class="fa-solid fa-file-lines text-blue-400 text-5xl"></i>
            <div>
                <input type="text" v-model="inputText" class="font-semibold text-lg">
                <p v-if="loading"><i class="fa-solid fa-rotate"></i> Saving changes</p>              
                <p v-else><i class="fa-solid fa-check"></i> Changes saved</p>              
            </div>

        </div>
        <div class="px-20 h-[60rem]">
            <textarea placeholder="Type something..." v-model="textarea"
            class="p-20 w-full h-full shadow resize-none outline-none"></textarea>
        </div>        
    </div>
    <div v-else>
        Home: No user is logged in
    </div>

</template>
<script>
import { onMounted, ref, watch } from 'vue';
import { doc, setDoc, getDoc, onSnapshot, updateDoc } from "firebase/firestore"; 
import {db, auth} from '../firebase/config'
import { getAuth, onAuthStateChanged } from "firebase/auth";

export default{
    setup() {
        const textarea = ref();
        const inputText = ref();
        const loading = ref(false)
        const userID = ref(null);
        const userData = ref(null);
        // const started = ref(false);

        watch(textarea, (currentValue, oldValue)=>{
            if(currentValue != oldValue && currentValue != userData.value.textareaMsg){
                loading.value = true;

                let temp = textarea.value;

                setTimeout(async()=>{
                    if(temp === textarea.value){
                        await updateDoc(doc(db, "textareaUser", userID.value), {
                            textareaMsg: textarea.value
                        })
                        console.log("textarea\n",textarea.value);
                        loading.value = false;
                    }
                }, 5000)
            }
        })

        watch(inputText, (currentValue, oldValue)=>{
            if(currentValue != oldValue && currentValue != userData.value.inputTitle){
                loading.value = true;

                let temp = inputText.value;

                setTimeout(async()=>{
                    if(temp === inputText.value){
                        await updateDoc(doc(db, "textareaUser", userID.value), {
                            inputTitle: inputText.value
                        })
                        console.log("title\n", inputText.value);
                        loading.value = false;
                    }
                }, 5000)
            }
        })

        onMounted(()=>{
            const unsub = onAuthStateChanged(auth, async(user) => {
                if(user){
                    await onSnapshot(doc(db, "textareaUser", user.uid), doc=>{
                        textarea.value = doc.data().textareaMsg;
                        inputText.value = doc.data().inputTitle;
                        userID.value = doc.id;
                        
                        userData.value = {...doc.data(), id: doc.id}

                        console.log(userData.value)
                    })
                } else{
                    console.log("user is not logged in")
                }
                unsub();
            })
        })
        // onMounted(async ()=>{
        //     await onSnapshot(doc(db, "textareaUser", "2TAbTRkhTxLqFjDQ30m0"), doc=>{
        //         textarea.value = doc.data().textareaMsg;
        //         inputText.value = doc.data().inputTitle;
        //     })
        // })

        return {textarea, inputText, loading, userData}
    }
}
</script>