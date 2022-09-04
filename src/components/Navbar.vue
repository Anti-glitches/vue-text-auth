<template>
    <div
        class="mx-6 p-4 font-semibold text-white flex justify-between gap-2 border-b-2"
    >
        <router-link
            to="/profile"
            class="mr-12 uppercase bg-blue-400 rounded-full w-12 h-12 flex justify-center items-center font-bold text-1xl border"
        >
            <span v-if="nameInitial">{{ nameInitial }}</span>
            <i v-else class="fa-solid fa-user"></i>
        </router-link>
        <router-link
            to="/"
            class="flex justify-center items-center text-purple-600 w-12 h-12 gap-2"
        >
            <i
                class="fa-solid fa-file-lines text-3xl text-transparent bg-clip-text bg-gradient-to-r from-blue-400 to-purple-400"
            ></i>
        </router-link>
        <!-- <router-link to="/dashboard">Dashboard</router-link> -->
        <div v-if="!loggedIn" class="flex items-center">
            <router-link
                to="/login"
                class="py-2 px-8 bg-green-500 border rounded-full shadow cursor-pointer"
                >Login</router-link
            >
        </div>
        <div
            v-else-if="loggedIn"
            @click="handleClick"
            class="flex justify-center items-center py-2 px-8 bg-red-500 border rounded-full shadow cursor-pointer"
        >
            <p>Logout</p>
        </div>
    </div>
</template>

<script>
import { onAuthStateChanged, signOut } from "firebase/auth";
import { db, auth } from "../firebase/config";
import { doc, getDoc } from "firebase/firestore";
import { onMounted, ref } from "vue";

import { useRouter } from "vue-router";

export default {
    setup() {
        const loggedIn = ref(null);
        const nameInitial = ref(null);

        const router = useRouter();

        const handleClick = () => {
            signOut(auth)
                .then(() => {
                    console.log("Logging you out", auth.currentUser);
                    location.reload();
                })
                .catch((err) => {
                    console.log(err);
                });
        };

        onMounted(() => {
            onAuthStateChanged(auth, async (user) => {
                if (user) {
                    loggedIn.value = true;
                    const docSnap = await getDoc(doc(db, "users", user.uid));

                    let { ["first name"]: firstName, ["last name"]: lastName } =
                        docSnap.data();

                    nameInitial.value = firstName[0] + lastName[0];
                } else {
                    loggedIn.value = false;
                }
            });
        });

        return { handleClick, auth, loggedIn, nameInitial };
    },
};
</script>
