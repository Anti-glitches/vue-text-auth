<template>
    <div v-if="!pageLoading">
        <div v-if="isLoggedin">
            <div v-if="fbUserTxtDoc && selectedDoc === null" class="m-6">
                <div @click="handleNew" class="flex items-center gap-3 p-3 px-8 w-40 border text-white
                cursor-pointer rounded-full bg-gradient-to-r from-yellow-400 to-orange-500">
                    <i class="fa-solid fa-file-circle-plus text-2xl"></i>
                    <span class="text-lg font-semibold">New</span>
                </div>
                <div class="grid sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6 cursor-pointer mt-6">
                    <div v-for="(doc, i) in fbUserTxtDoc" :key="i" @click="handleClick(doc, i)"
                        class="w-full border flex gap-4 items-center bg-slate-100 p-3 px-4 rounded-lg shadow">
                            <i class="fa-solid fa-file-lines text-2xl text-blue-400"></i>
                            <p class="text-base font-medium">{{doc.inputTitle}}</p>
                    </div>
                </div>
            </div>
            <div v-if="selectedDoc" class="bg-slate-100">
                <div class="bg-white border-b-2 px-24 py-8 mb-4 flex gap-3 justify-between">
                    <i class="fa-solid fa-file-lines text-blue-400 text-5xl"></i>
                    <div class="flex-grow">
                        <input type="text" v-model="selectedInputTitle" class="font-semibold text-lg">
                        <p v-if="loading"><i class="fa-solid fa-rotate"></i> Saving changes</p>              
                        <p v-else><i class="fa-solid fa-check"></i> Changes saved</p>              
                    </div>
                    <div @click="handleHome" class="border flex justify-center items-center p-4 rounded-lg shadow cursor-pointer min-w-fit">
                        <i class="fa-solid fa-angle-left mr-2"></i>
                        <span class="font-semibold">Dashboard</span>
                    </div>
                </div>
                <div class="px-20 h-[60rem]">
                    <textarea placeholder="Type something..." v-model="selectedTextarea"
                    class="p-20 w-full h-full shadow resize-none outline-none"></textarea>
                </div>  
            </div>
        </div>
        <div v-else>
            Dashboard: No user is logged in
        </div>
    </div>
    <div v-else>
        <Loading/>
    </div>
</template>
<script>
import { onMounted, ref, watch } from 'vue';
import { doc, getDoc, updateDoc } from "firebase/firestore"; 
import {db, auth} from '../firebase/config'
import { onAuthStateChanged, getAuth } from "firebase/auth";
import Loading from './Loading.vue';

export default {
    setup() {
        const fbUserTxtDoc = ref(null);
        const selectedDoc = ref(null);
        const loading = ref(false);
        const selectedTextarea = ref(null);
        const selectedInputTitle = ref(null);
        const id = ref(null);
        const isLoggedin = ref(false);
        const userID = ref(null);
        const pageLoading = ref(true);

        onMounted(async () => {
            const unsub = onAuthStateChanged(auth, async (user) => {
                if (user) {
                    const docSnap = await getDoc(doc(db, "textareaUser", user.uid));
                    fbUserTxtDoc.value = docSnap.data().userTxtDoc;
                    isLoggedin.value = true;
                    userID.value = user.uid;
                    console.log(fbUserTxtDoc.value, userID.value);
                }
                else {
                    console.log("user is not logged in");
                }
                unsub();
                pageLoading.value = false;
            });
        });
        const handleClick = (doc, i) => {
            selectedDoc.value = doc;
            selectedTextarea.value = doc.textarea;
            selectedInputTitle.value = doc.inputTitle;
            id.value = i;
        };
        const handleHome = () => {
            selectedDoc.value = null;
            id.value = null;
        };
        const handleNew = async () => {
            const data = {
                inputTitle: "new document",
                textarea: ""
            };
            fbUserTxtDoc.value.push(data);
            handleClick(data, fbUserTxtDoc.value.length - 1);
            await updateDoc(doc(db, "textareaUser", userID.value), {
                userTxtDoc: fbUserTxtDoc.value
            });
        };
        watch(selectedTextarea, (currentValue, oldValue) => {
            if (currentValue != oldValue && currentValue != selectedDoc.value.textarea) {
                loading.value = true;
                console.log("changes");
                let temp = selectedTextarea.value;
                setTimeout(async () => {
                    if (temp === selectedTextarea.value) {
                        fbUserTxtDoc.value[id.value].textarea = selectedTextarea.value;
                        await updateDoc(doc(db, "textareaUser", userID.value), {
                            userTxtDoc: fbUserTxtDoc.value
                        });
                        console.log("textarea\n", selectedTextarea.value);
                        loading.value = false;
                    }
                }, 5000);
            }
        });
        watch(selectedInputTitle, (currentValue, oldValue) => {
            if (currentValue != oldValue && currentValue != selectedDoc.value.inputTitle) {
                loading.value = true;
                console.log("changes input");
                let temp = selectedInputTitle.value;
                setTimeout(async () => {
                    if (temp === selectedInputTitle.value) {
                        fbUserTxtDoc.value[id.value].inputTitle = selectedInputTitle.value;
                        await updateDoc(doc(db, "textareaUser", userID.value), {
                            userTxtDoc: fbUserTxtDoc.value
                        });
                        console.log("input title\n", selectedInputTitle.value);
                        loading.value = false;
                    }
                }, 1000);
            }
        });
        return {
            fbUserTxtDoc,
            selectedDoc,
            selectedTextarea,
            selectedInputTitle,
            isLoggedin,
            loading,
            pageLoading,
            handleClick,
            handleHome,
            handleNew
        };
    },
    components: { Loading }
}
</script>