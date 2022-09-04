<template>
    <div class="p-8 shadow rounded-md">
        <div class="flex flex-col items-center mb-8 pb-4 border-b-2">
            <i
                class="fa-solid fa-file-lines mr-3 text-4xl text-transparent bg-clip-text bg-gradient-to-r from-blue-400 to-purple-400"
            ></i>
            <h1
                class="py-2 font-extrabold text-transparent text-3xl bg-clip-text bg-gradient-to-r from-blue-400 to-purple-400"
            >
                Google Docs Clone
            </h1>
        </div>

        <div class="flex flex-col items-center">
            <h1 class="text-2xl font-semibold">
                You're currently not logged in
            </h1>
            <p class="mt-2">Please login/register 👇</p>
        </div>

        <div class="flex flex-col items-center gap-1 mt-4">
            <button
                @click="$router.push('/login')"
                class="w-full p-2 font-semibold text-white bg-green-500 border-2 border-green-500 rounded-full shadow"
            >
                Login
            </button>
            <p class="font-bold">or</p>
            <button
                @click="$router.push('/register')"
                class="w-full p-2 font-semibold text-green-500 border-2 border-green-500 rounded-full shadow"
            >
                Register
            </button>
        </div>
    </div>
</template>
<script>
export default {};
</script>
