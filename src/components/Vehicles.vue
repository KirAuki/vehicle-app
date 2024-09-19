<template>
    <section class="vehicles">
        <div class="vehicles__controls">
            <div class="vehicles__search">
                <input type="text" v-model="searchQuery" placeholder="Search VIN" @input="fetchCars" />
                <img src="../assets/icons/search.svg" alt="" />
            </div>

            <div class="vehicles__per-page">
                <p>Select vehicles per page:</p>
                <select v-model="perPage" @change="fetchCars">
                    <option v-for="option in perPageOptions" :key="option" :value="option">
                        {{ option }}
                    </option>
                </select>
            </div>
            <button class="vehicles__add-btn">
                <img src="../assets/icons/plus.svg" alt="" />
                ADD VEHICLE
            </button>
        </div>
        <ul v-if="vehicles.length > 0" class="vehicles__list">
            <li v-for="vehicle in vehicles" :key="vehicle.id" class="vehicles__list-item">
                <img src="../assets/icons/dots.svg" alt="" class="vehicles__dots" />
                <img :src="vehicle.placeholder" alt="Vehicle" class="vehicles__image" />
                <h3 class="vehicles__name">{{ vehicle.vehicle_name || 'Noname' }}</h3>
                <p class="vehicles__vin">{{ vehicle.vin }}</p>
                <hr />
                <div class="vehicles__status">
                    <div class="vehicles__angles" :class="{ 'full-angles': vehicle.angles_count === 30 }">
                        <img v-show="vehicle.angles_count === 30" src="../assets/icons/check.svg" alt="check" />
                        <span>{{ vehicle.angles_count }}/30</span>
                    </div>
                    <p v-if="vehicle.created_at" class="vehicles__date">
                        {{ new Date(vehicle.created_at * 1000).toLocaleDateString() }}
                    </p>
                </div>
            </li>
        </ul>
        <span v-else class="vehicles__missing"> Vehicles missing</span>
        <div class="vehicles__pagination">
            <p class="vehicles__count">
                Showing {{ (page - 1) * perPage + vehicles.length }} out of {{ totalVehicles }}
            </p>
            <div class="vehicles__pagination-controls">
                <button @click="prevPage" :disabled="page === 1" class="vehicles__pagination-btn">
                    <img src="../assets/icons/chevron_left.svg" alt="previous" />
                </button>
                <span class="vehicles__pagination-page">{{ page }}</span>
                <span>of</span>
                <span class="vehicles__pagination-page">{{ totalPages }}</span>
                <button @click="nextPage" :disabled="page === totalPages" class="vehicles__pagination-btn">
                    <img src="../assets/icons/chevron_right.svg" alt="next" />
                </button>
            </div>
        </div>
    </section>
</template>


<script>
import { defineComponent, ref, onMounted } from 'vue';
import axios from 'axios';

export default defineComponent({
    setup() {
        const vehicles = ref([]);
        const page = ref(1);
        const perPage = ref(9);
        const searchQuery = ref('');
        const totalPages = ref(1);
        const totalVehicles = ref(0);
        const perPageOptions = [6, 9, 12];

        const fetchCars = async () => {
            try {
                const response = await axios.get('https://api.caiman-app.de/api/cars-test', {
                    params: {
                        search: searchQuery.value || '',
                        per_page: perPage.value,
                        page: page.value,
                    },
                });
                vehicles.value = response.data.data;
                totalPages.value = response.data.meta.last_page;
                totalVehicles.value = response.data.meta.total;
            } catch (error) {
                console.error('Failed to fetch vehicles:', error);
            }
        };

        const prevPage = () => {
            if (page.value > 1) {
                page.value--;
                fetchCars();
            }
        };

        const nextPage = () => {
            if (page.value < totalPages.value) {
                page.value++;
                fetchCars();
            }
        };

        onMounted(() => {
            fetchCars();
        });

        return {
            vehicles,
            page,
            perPage,
            searchQuery,
            totalVehicles,
            totalPages,
            perPageOptions,
            fetchCars,
            prevPage,
            nextPage,
        };
    },
});
</script>



<style lang="scss">
.vehicles {
    &__controls {
        display: flex;
        flex-direction: row;
        gap: 32px;
        width: 100%;
        height: 42px;
        margin: 34px 0;
        justify-content: space-between;
    }
    &__search {
        position: relative;
        display: inline-block;
        height: 42px;
        & input {
            font-size: 15px;
            width: 318px;
            padding: 0 16px;
            border: 1px solid #e4e4e4;
            border-radius: 8px;
            height: 100%;
        }
        & img {
            width: 24px;
            height: 24px;
            position: absolute;
            top: 25%;
            right: 12px;
        }
    }
    &__per-page {
        display: flex;
        flex-direction: row;
        align-items: center;
        gap: 16px;
        & select {
            padding: 9px 16px;
            border-radius: 8px;
            border: solid 1px #e4e4e4;
        }
    }
    &__add-btn {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 16px;
        border-radius: 8px;
        padding: 9px 16px;
        border: 1px solid #d90e32;
        background-color: #d90e32;
        color: #ffffff;
        &:hover,
        &:focus {
            opacity: 0.8;
        }
    }
    &__list {
        display: grid;
        grid-template-columns: repeat(3, minmax(200px, 0.5fr));
        gap: 30px;
        font-size: 15px;
        font-weight: bold;
    }
    &__missing {
        display: flex;
        width: 100%;
        justify-content: center;
        text-align: center;
    }
    &__list-item {
        display: flex;
        flex-direction: column;
        padding: 16px 24px;
        background-color: #f3f6f8;
        border-radius: 10px;
        &:hover {
            transform: scale(1.01);
        }
    }
    &__dots {
        width: 24px;
        height: 24px;
        align-self: flex-end;
        cursor: pointer;
    }
    &__image {
        max-width: 85%;
        align-self: center;
    }
    &__name {
        font-size: 20px;
        margin-top: 24px;
    }
    &__vin {
        color: rgba(41, 49, 72, 0.6);
        margin-top: 12px;
        font-weight: 500;
    }
    hr {
        color: #e4e4e4;
        border: solid 1px #e4e4e4;
        width: 100%;
        margin-top: 18px;
    }
    &__status {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-top: 18px;
    }
    &__angles {
        background-color: #ededed;
        padding: 8px 14px;
        border-radius: 6px;
    }
    &__date {
        color: #29314899;
    }
    &__pagination {
        display: flex;
        justify-content: space-between;
        margin-top: 30px;
        align-items: center;
        &-controls {
            display: flex;
            gap: 12px;
            align-items: center;
        }
        &-btn {
            display: flex;
            justify-content: center;
            align-items: center;
            width: 24px;
            height: 24px;
            border: unset;
            background-color: transparent;
            cursor: pointer;
            &:hover,
            &:focus {
                opacity: 0.8;
            }
        }
        &-page {
            display: flex;
            width: 32px;
            height: 32px;
            border: 1px solid #e4e4e4;
            justify-content: center;
            align-items: center;
            border-radius: 6px;
        }
    }
    & .full-angles {
        background-color: #7fc75e;
    }
}
</style>
