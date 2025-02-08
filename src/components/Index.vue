<template>
    <!-- header -->
    <div class="max-w-7xl mx-auto sm:px-6 lg:px-8 p-5 sm:p-0">
        <div class="flex items-center justify-between">
            <div class="py-4">
                <img v-bind:src="require('@/assets/Logo.png')" alt="Example Image" style="height: 50px;" />
            </div>
            <div>
                <BellOutlined class="ml-7 cursor-pointer text-xl" />
                <SettingOutlined class="ml-7 cursor-pointer text-xl" />
                <LogoutOutlined class="ml-7 cursor-pointer text-xl" />
            </div>
        </div>
    </div>

    <!-- user details setion -->
    <div class="bg-cyan-500 pb-6 pt-8">
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8 p-5 sm:p-0 container mx-auto flex items-center">
            <div class="relative">
                <div class="flex pt-12 items-end" style="margin-left: 180px;">
                    <div class="mr-6 px-1">
                        <h2 class="text-white text-4xl font-semibold">John Doe</h2>
                        <small class="text-white mt-2 block">Last online: 2 days ago</small>
                    </div>
                    <div class="flex" style="height: 45px;">
                        <button
                            class="border border-white bg-white text-cyan-400 font-semibold rounded-md flex items-center mr-2 px-6 py-3 text-sm">
                            <SendOutlined class="mr-4" />
                            Send Message
                        </button>
                        <button
                            class="border border-white text-white font-semibold rounded-md flex items-center mr-2 px-6 py-3 text-sm">
                            <PlusOutlined class="mr-4" />
                            Add Friend
                        </button>
                    </div>
                    <!-- Avatar Image positioned below the background -->
                    <div class="absolute top-4 left-0 w-40 h-40 z-0">
                        <img v-bind:src="require('@/assets/Avatar.png')" alt="Example Image"
                            class="w-full h-full object-cover" />
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- list data section -->
    <div class="max-w-7xl mx-auto sm:px-6 lg:px-8 p-5 sm:p-0 container mx-auto flex items-center mt-14">
        <ListComponent apiUrl="https://randomuser.me/api" class="w-full">
            <template v-slot:list-view="{ data, apiResponse, fullApiResponse }">
                <!-- {{ data }} -->
                <UsersListTemplate v-for="product in data" :key="product.id" :data="product" />
            </template>
        </ListComponent>
    </div>
</template>

<script setup>
import { SettingOutlined, BellOutlined, LogoutOutlined, SendOutlined, PlusOutlined } from '@ant-design/icons-vue';
import ListComponent from './General/ListComponent.vue';
import UsersListTemplate from './List/UsersListTemplate.vue';
</script>
