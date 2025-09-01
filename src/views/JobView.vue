<script setup>
import { reactive, onMounted } from 'vue';
import { useRoute, RouterLink } from 'vue-router';
import PulseLoader from 'vue-spinner/src/PulseLoader.vue';
import axios from 'axios';
import BackButton from '@/components/BackButton.vue';

const route = useRoute();

const jobId = route.params.id;

const state = reactive({
    job: {},
    isLoading: true
})

onMounted(async () => {
    try {
        const response = await axios.get(`http://localhost:5000/jobs/${jobId}`);
        state.job = response.data;
    } catch (error) {
        console.error('error:', error);
    } finally {
        state.isLoading = false;
    }
})
</script>

<template>
    <BackButton />
    <section v-if="!state.isLoading" class="bg-green-50 min-h-screen py-10 px-6">
        <div class="container mx-auto">
            <div class="grid grid-cols-1 md:grid-cols-[70%_30%] gap-6">
                <!-- Main Content -->
                <main>
                    <!-- Job Header -->
                    <div class="bg-white p-6 rounded-2xl shadow-md text-center md:text-left">
                        <div class="text-gray-500 mb-2">{{ state.job.type }}</div>
                        <h1 class="text-3xl font-bold mb-3">{{ state.job.title }}</h1>
                        <div class="flex items-center justify-center md:justify-start text-orange-800">
                            <i class="pi pi-map-marker mr-2"></i>
                            <p>{{ state.job.location }}</p>
                        </div>
                    </div>

                    <!-- Job Description -->
                    <div class="bg-white p-6 rounded-2xl shadow-md mt-6">
                        <h3 class="text-green-800 text-xl font-bold mb-4">Job Description</h3>
                        <p class="text-gray-700 mb-6">{{ state.job.description }}</p>

                        <h3 class="text-green-800 text-xl font-bold mb-2">Salary</h3>
                        <p class="text-gray-800 font-semibold">{{ state.job.salary }} / Year</p>
                    </div>
                </main>

                <!-- Sidebar -->
                <aside class="space-y-6">
                    <!-- Company Info -->
                    <div class="bg-white p-6 rounded-2xl shadow-md">
                        <h3 class="text-xl font-bold mb-4">Company Info</h3>
                        <h2 class="text-2xl font-semibold mb-2">{{ state.job.company.name }}</h2>
                        <p class="text-gray-600 mb-4">
                            {{ state.job.company.description }}
                        </p>

                        <hr class="my-4" />

                        <div class="mb-3">
                            <h3 class="text-lg font-semibold">Contact Email:</h3>
                            <p class="bg-green-50 p-2 mt-1 rounded font-bold text-gray-800">
                                {{ state.job.company.contactEmail }}
                            </p>
                        </div>

                        <div>
                            <h3 class="text-lg font-semibold">Contact Phone:</h3>
                            <p class="bg-green-50 p-2 mt-1 rounded font-bold text-gray-800">
                                {{ state.job.company.contactPhone }}
                            </p>
                        </div>
                    </div>

                    <!-- Manage Job -->
                    <div class="bg-white p-6 rounded-2xl shadow-md">
                        <h3 class="text-xl font-bold mb-4">Manage Job</h3>
                        <RouterLink :to="`/jobs/edit/${state.job.id}`"
                            class="block w-full text-center bg-green-700 hover:bg-green-600 text-white font-bold py-2 px-4 rounded-full">
                            Edit Job
                        </RouterLink>
                        <button
                            class="block w-full mt-4 bg-red-800 hover:bg-red-700 text-white font-bold py-2 px-4 rounded-full">
                            Delete Job
                        </button>
                    </div>
                </aside>
            </div>
        </div>
    </section>

    <div v-else class="text-center text-gray-500 py-6">
        <PulseLoader />
    </div>
</template>
