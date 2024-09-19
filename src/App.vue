<script setup>
import Vehicles from "./components/Vehicles.vue"
import Header from "./components/Header.vue"
import Navbar from "./components/Navbar.vue";
</script>

<template>
    <div class="screen">
        <Navbar />
        <div class="screen__content">
            <Header/>
            <Vehicles/>
        </div>
    </div>
</template>

<style lang="scss">
.screen {
    display: flex;
    flex-direction: row;
    min-height: 100vh;
    height: 100%;
    &__content {
        padding: 0 30px 50px;
        width: 100%;
    }
}
</style>
