<template>
    <div>
        <div v-if="isLoading">
            <LoadingComponent class="mt-20 mb-20" />
        </div>
        <div v-else>
            <div v-if="apiResponse">
                <small class="text-right block mb-4">Showing <b>{{ limit }}</b> Per Page.</small>
                <div class="grid md:grid-cols gap-4 mb-5">
                    <slot name="list-view" :data="listData" :apiResponse="apiResponse"
                        :fullApiResponse="fullApiResponse" />
                </div>
                <div v-if="listData.length === 0">
                    <NotFound />
                </div>
                <div class="flex justify-between w-full pt-6 flex-col sm:flex-row">
                    <div class="flex justify-start flex-1">
                        <button class="bg-cyan-400 hover:bg-gray-200 text-white py-2 px-4 rounded border flex items-center" @click="prevPage"><ReloadOutlined class="mr-3" />Refresh</button>
                    </div>
                    <div class="mb-4 sm:mb-0 flex items-end">
                        <small class="text-center">Current Page: <b>{{ currentPage }}</b></small>
                    </div>
                    <div class="flex justify-end flex-1">
                        <button class="bg-white hover:bg-gray-200 text-gray py-2 px-4 rounded ml-2 border" @click="prevPage" :disabled="currentPage === 1" :class="{ 'cursor-not-allowed': currentPage === 1 }">Previous Page</button>
                        <button class="bg-black hover:bg-gray-700 text-white py-2 px-4 rounded ml-2" @click="nextPage">Next Page</button>
                    </div>
                </div>
            </div>
            <div v-else>
                <NotFound class="not-found" />
            </div>
        </div>
    </div>
</template>

<script setup>
import axios from 'axios';
import { ref, onMounted, watch, computed } from 'vue';
import LoadingComponent from './LoadingComponent.vue';
import NotFound from '../Icons/NotFound.vue';
import Swal from 'sweetalert2';
import { ReloadOutlined } from '@ant-design/icons-vue';

const isLoading = ref(true);
const listData = ref([]);
const filters = ref([]);
const error = ref(null);
const searchText = ref('');
const apiResponse = ref(null);
const fullApiResponse = ref(null);
const limit = ref(7);
const currentPage = ref(1);
const lastPage = ref(0);
const selectAll = ref(false);
const allIds = ref([]);
const props = defineProps({
    apiUrl: {
        type: String,
        required: true,
    },
});

// Watch the createCompleteSignal prop and fetch data if true
watch(() => props.createCompleteSignal, (newValue) => {
    if (newValue) {
        fetchList(currentPage.value);
    }
});

const fetchList = async (page = 1) => {
    isLoading.value = true;
    try {
        const queryParams = new URLSearchParams({
            results: limit.value,
            page: page,
        });

        const { data } = await axios.get(`${props.apiUrl}?${queryParams.toString()}`);

        listData.value = data?.results || [];

        apiResponse.value = {
            from: data.info.page * limit.value - (limit.value - 1),
            to: data.info.page * limit.value,
            total: data.info.results,
            ...data.info
        };
        fullApiResponse.value = data;
        lastPage.value = Math.ceil(data.info.results / limit.value);

    } catch (err) {
        error.value = err;
    } finally {
        isLoading.value = false;
    }
};

// On component mount, load data
onMounted(() => {
    fetchList(currentPage.value);
});

// Pagination functions
const nextPage = () => {
    console.log('clicked');
    // if (currentPage.value >= lastPage.value) return;
    currentPage.value += 1;
    fetchList(currentPage.value);
};

const prevPage = () => {
    if (currentPage.value <= 1) return;
    currentPage.value -= 1;
    fetchList(currentPage.value);
};
</script>
