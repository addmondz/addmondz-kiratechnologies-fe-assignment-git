<template>
    <div class="w-full flex flex-col justify-center order-last lg:order-none max-lg:mx-auto border p-5 rounded-xl overflow-hidden bg-white hover:border-cyan-600 transition-all duration-500 cursor-pointer"
        @click="toggleModal">
        <div class="flex">
            <div class="flex-1">
                <div class="grid lg:grid-cols-5 grid-cols-2 gap-x-4 gap-y-4">
                    <div class="col">
                        <div class="row">
                            <div class="col-auto">
                                <div class="mb-1 text-xs text-gray-500">DOB</div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col">
                                <div class="text-[14px]">{{ formatDate(data.dob.date) }}</div>
                            </div>
                        </div>
                    </div>
                    <div class="col">
                        <div class="row">
                            <div class="col-auto">
                                <div class="mb-1 text-xs text-gray-500">Name</div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col">
                                <div class="text-[14px] truncate">{{ data.name.first }} {{ data.name.last }}</div>
                            </div>
                        </div>
                    </div>
                    <div class="col">
                        <div class="row">
                            <div class="col-auto">
                                <div class="mb-1 text-xs text-gray-500">Gender</div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col">
                                <div class="text-[14px] capitalize">{{ data.gender }}</div>
                            </div>
                        </div>
                    </div>
                    <div class="col">
                        <div class="row">
                            <div class="col-auto">
                                <div class="mb-1 text-xs text-gray-500">Country</div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col">
                                <div class="text-[14px]">{{ data.location.country ?? '-' }}</div>
                            </div>
                        </div>
                    </div>
                    <div class="col">
                        <div class="row">
                            <div class="col-auto">
                                <div class="mb-1 text-xs text-gray-500">Email</div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col">
                                <div class="text-[14px] capitalize">{{ data.email }}</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <Modal :show="showingModal" @close="toggleModal">
        <div class="bg-white p-9">
            <div class="modal-content">
                <div class="modal-header mb-4">
                    <h2 class="modal-title text-4xl font-bold">{{ data.name.first }} {{ data.name.last }}</h2>
                </div>
                <div class="modal-body">
                    <div class="modal-row flex justify-between mb-2">
                        <span class="modal-label text-gray-500 w-1/3">DOB:</span>
                        <span class="modal-value w-2/3 text-left">{{ formatDate(data.dob.date) }}</span>
                    </div>
                    <div class="modal-row flex justify-between mb-2">
                        <span class="modal-label text-gray-500 w-1/3">Status:</span>
                        <span class="modal-value w-2/3 text-left">{{ data.status ?? 'N/A' }}</span>
                    </div>
                    <div class="modal-row flex justify-between mb-2">
                        <span class="modal-label text-gray-500 w-1/3">Gender:</span>
                        <span class="modal-value w-2/3 text-left capitalize">{{ data.gender }}</span>
                    </div>
                    <div class="modal-row flex justify-between mb-2">
                        <span class="modal-label text-gray-500 w-1/3">Country:</span>
                        <span class="modal-value w-2/3 text-left">{{ data.location.country ?? '-' }}</span>
                    </div>
                    <div class="modal-row flex justify-between mb-2">
                        <span class="modal-label text-gray-500 w-1/3">Email:</span>
                        <span class="modal-value w-2/3 text-left">{{ data.email }}</span>
                    </div>
                </div>
            </div>
        </div>
    </Modal>
</template>

<script setup>
import Modal from '../General/Modal.vue';
import { ref } from 'vue';

const props = defineProps({
    data: {
        type: Object,
        required: true,
    },
});

const showingModal = ref(false);

const toggleModal = () => {
    showingModal.value = !showingModal.value;
};

const formatDate = (date) => {
    // Ensure that date is a JavaScript Date object
    const validDate = new Date(date);
    if (isNaN(validDate)) {
        return '-'; // Return a placeholder if the date is invalid
    }

    const options = { year: 'numeric', month: 'short', day: '2-digit' };
    return validDate.toLocaleDateString('en-GB', options).replace(/^(\d{2}) (\w{3}) (\d{4})$/, '$1 $2 $3');
};
</script>
