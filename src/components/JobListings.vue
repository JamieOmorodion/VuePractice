<script setup>
import { ref, onMounted } from 'vue'
import JobListing from './JobListing.vue'
import { defineProps } from 'vue'
import { RouterLink } from 'vue-router'
import axios from 'axios'
import PulseLoader from 'vue-spinner/src/PulseLoader.vue'

const jobs = ref([])
const isLoading = ref(true)
console.log(jobs.value)

defineProps({
  limit: Number,
  showBtn: {
    type: Boolean,
    default: false,
  },
})

onMounted(async () => {
  try {
    const response = await axios.get('http://localhost:5000/jobs')
    jobs.value = response.data
  } catch (error) {
    console.error('Error fetching jobs', error)
  } finally {
    isLoading.value = false
  }
})
</script>

<template>
  <section class="bg-blue-50 px-4 py-10">
    <div class="container-xl lg:container m-auto">
      <h2 class="text-3xl font-bold text-green mb-6 text-center">Browse Jobs</h2>
      <!-- Show loading spinner while loading is true -->
      <div v-if="isLoading" class="text-center text-gray-500 py-6">
        <PulseLoader />
      </div>
      <!-- Show job listing when done loading -->
      <div v-else class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <JobListing v-for="job in jobs.slice(0, limit || jobs.length)" :key="job.id" :job="job" />
      </div>
    </div>
  </section>
  <section v-if="showBtn" class="m-auto max-w-lg my-10 px-6">
    <RouterLink
      to="/jobs"
      class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
      >View All Jobs</RouterLink
    >
  </section>
</template>
