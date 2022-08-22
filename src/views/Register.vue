<template lang="">
    <div>
        <h1 class="font-bold">Register</h1>
        <form @submit.prevent="handleClick">
            <input type="text" placeholder="first name" class="w-full border">
            <input type="text" placeholder="last name" class="w-full border">
            <input type="number" placeholder="age" min="0" class="w-full border">
            <input type="email" placeholder="email" class="w-full border">
            <input type="password" placeholder="password" class="w-full border">
            <button class="p-3 bg-emerald-300">Register</button>
        </form>
    </div>
</template>
<script>
export default {
    setup() {
        const handleClick = () =>{
            console.log('hello')
        }

        return {handleClick}
    }
}
</script>