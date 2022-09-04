<template lang="">
    <div class="mx-6">
        <div>
            <h1 class="font-bold text-2xl py-4">Login</h1>
            <form @submit.prevent="handleClick" class="flex flex-col gap-3">
                <!-- <input type="email" placeholder="email" v-model="email" class="w-full border"> -->
                <div class="relative">
                    <input
                        v-model="email"
                        type="email"
                        id="email"
                        class="block rounded-t-lg px-2.5 pb-2.5 pt-5 w-full text-sm text-gray-900 bg-gray-50 dark:bg-gray-700 border-0 border-b-2 border-gray-300 appearance-none dark:text-white dark:border-gray-600 dark:focus:border-blue-500 focus:outline-none focus:ring-0 focus:border-blue-600 peer"
                        placeholder=" "
                    />
                    <label
                        for="email"
                        class="absolute text-sm text-gray-500 dark:text-gray-400 duration-300 transform -translate-y-4 scale-75 top-4 z-10 origin-[0] left-2.5 peer-focus:text-blue-600 peer-focus:dark:text-blue-500 peer-placeholder-shown:scale-100 peer-placeholder-shown:translate-y-0 peer-focus:scale-75 peer-focus:-translate-y-4"
                        >Email</label
                    >
                </div>
                <div class="relative">
                    <input
                        v-model="password"
                        type="password"
                        id="password"
                        class="block rounded-t-lg px-2.5 pb-2.5 pt-5 w-full text-sm text-gray-900 bg-gray-50 dark:bg-gray-700 border-0 border-b-2 border-gray-300 appearance-none dark:text-white dark:border-gray-600 dark:focus:border-blue-500 focus:outline-none focus:ring-0 focus:border-blue-600 peer"
                        placeholder=" "
                    />
                    <label
                        for="password"
                        class="absolute text-sm text-gray-500 dark:text-gray-400 duration-300 transform -translate-y-4 scale-75 top-4 z-10 origin-[0] left-2.5 peer-focus:text-blue-600 peer-focus:dark:text-blue-500 peer-placeholder-shown:scale-100 peer-placeholder-shown:translate-y-0 peer-focus:scale-75 peer-focus:-translate-y-4"
                        >Password</label
                    >
                </div>
                <!-- <input type="password" placeholder="password" v-model="password" class="w-full border"> -->
                <button
                    class="p-3 bg-emerald-400 font-semibold text-white rounded"
                >
                    Login
                </button>
            </form>
        </div>
        <div>
            <router-link to="/register" class="underline"
                >Don't have an account?</router-link
            >
        </div>
    </div>
</template>
<script>
import { signInWithEmailAndPassword } from "firebase/auth";
import { ref, onMounted } from "vue";
import { doc, setDoc, getDoc } from "firebase/firestore";
import { db, auth } from "../firebase/config";

import { useRouter } from "vue-router";

export default {
    setup() {
        const email = ref();
        const password = ref();

        const router = useRouter();

        const handleClick = () => {
            signInWithEmailAndPassword(auth, email.value, password.value)
                .then((userCredential) => {
                    console.log("you are logged in", userCredential);

                    getDoc(doc(db, "users", userCredential.user.uid))
                        .then((doc) => {
                            console.log(doc.id, "=>", doc.data());

                            router.push("/");
                        })
                        .catch((err) => {
                            console.log(err);
                        });

                    email.value = password.value = "";
                })
                .catch((err) => {
                    console.log(err);
                });
        };

        return { email, password, handleClick };
    },
};
</script>
