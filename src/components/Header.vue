<template>
    <header class="header">
        <div class="header__left">
            <h2 class="header__title">Vehicles</h2>
            <span class="header__counter">256</span>
        </div>
        <div class="header__right">
            <button class="header__add"><img src="../assets/icons/plusred.svg" alt="plus" /></button>
            <div class="header__profile">
                <img src="../assets/profile.png" alt="profile image" />
                <a href="#">John Doe</a>
            </div>
            <div class="header__lang-switch">
                <label for="language"><img src="../assets/icons/uk.svg" alt="uk" /></label>
                <select name="language" id="language">
                    <option value="En">En</option>
                </select>
            </div>
        </div>
    </header>
</template>
<script setup></script>
<style lang="scss">
.header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 102px;
    width: 100%;

    &__left {
        display: flex;
        align-items: center;
        gap: 18px;
    }
    &__title {
        font-size: 2.5rem;
    }
    &__counter {
        background-color: #f3f6f8;
        padding: 4px 14px;
        border-radius: 6px;
    }
    &__right {
        display: flex;
        gap: 35px;
        align-items: center;
    }
    &__add {
        width: 42px;
        height: 42px;
        border: 1px solid #e86e84;
        background-color: transparent;
        cursor: pointer;
        &:hover,
        &:focus {
            opacity: 0.8;
        }
        & img {
            width: 24px;
            height: 24px;
        }
    }
    &__profile {
        display: flex;
        align-items: center;
        gap: 14px;
    }
    &__lang-switch {
        display: flex;
        gap: 14px;
        & select {
            border: unset;
            cursor: pointer;
            &:hover,
            &:focus {
                border: unset;
                opacity: 0.8;
            }
        }
    }
}
</style>
