<template>
    <nav class="navbar">
        <h1 class="navbar__title">Demo Test</h1>
        <ul class="navbar__list">
            <li class="navbar__item"><img src="../assets/icons/user.svg" alt="user" /><a href="#">Profile</a></li>
            <li class="navbar__item"><img src="../assets/icons/rim.svg" alt="rim" /><a href="#">Vehicles</a></li>
            <li class="navbar__item">
                <img src="../assets/icons/settings.svg" alt="settings" /><a href="#">Settings</a>
            </li>
        </ul>
    </nav>
</template>
<script setup></script>
<style lang="scss">
.navbar {
    background-color: #282828;
    font-size: 20px;
    width: 256px;
    color: #ffffff;
    display: flex;
    flex-direction: column;
    font-weight: 500;
    &__title {
        color: #ffffff;
        font-size: 1.5rem;
        margin-top: 30px;
        align-self: center;
    }
    &__list {
        margin-top: 102px;
        display: flex;
        flex-direction: column;
        gap: 16px;
    }
    &__item {
        display: flex;
        flex-direction: row;
        justify-content: start;
        padding: 12px 30px;
        align-items: center;
        gap: 20px;

        cursor: pointer;
        &:hover,
        &:focus {
            background-color: #ffffff33;
            border-left: solid 2px #d90e32;
        }
        a {
            color: #ffffff33;
        }
    }
}
</style>
