<script setup>
import { defineProps, ref, computed } from 'vue';
import { RouterLink } from 'vue-router';
const props = defineProps({
    job: Object
});

const showFullDescription = ref(false)

const toggelFullDescription = () => {
    showFullDescription.value = !showFullDescription.value
}
const truncatedDescription = computed(() => {
    let description = props.job.description;
    if (!showFullDescription.value) {
        description = description.substring(0, 90) + '...';
    }
    return description;
})
</script>

<template>
    <div class="bg-white rounded-xl shadow-md relative">
        <div class="p-4">
            <div class="mb-6">
                <div class="text-gray-500 my-2">
                    {{ job.type }}
                </div>
                <h3 class="text-xl font-bold">{{ job.title }}</h3>
            </div>

            <div class="mb-5">
                <div>
                    {{ truncatedDescription }}
                    <button @click="toggelFullDescription"
                        class="text-green-700 hover:text-green-800 cursor-pointer mb-5">
                        {{ showFullDescription ? 'Less' : 'More' }}
                    </button>
                </div>
            </div>

            <h3 class="text-green-700 font-bold mb-2">{{ job.salary }} / Year</h3>

            <div class="border border-gray-100 mb-5"></div>

            <div class="flex flex-col lg:flex-row justify-between mb-4">
                <div class="text-orange-800 mb-3">
                    <i class="pi pi-map-marker text-orange-800"></i>
                    {{ job.location }}
                </div>
                <RouterLink :to="'job/' + job.id"
                    class="h-[36px] bg-green-700 hover:bg-green-600 text-white px-4 py-2 rounded-lg text-center text-sm">
                    Read More
                </RouterLink>
            </div>
        </div>
    </div>
</template>