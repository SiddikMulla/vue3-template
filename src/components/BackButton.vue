<script setup>
import { RouterLink } from 'vue-router';

</script>

<template>
    <section>
        <div class="container m-auto py-6 px-6">
            <RouterLink to="/jobs" class="text-green-600 hover:text-green-600 text-xl flex items-center">
                <i class="pi pi-arrow-circle-left mr-2"></i> Back to Job Listings
            </RouterLink>
        </div>
    </section>

</template>