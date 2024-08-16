<template>
  <main class="container mx-auto my-8 space-y-8">
    <h1 class="text-4xl font-medium">Event Booking</h1>
    <h2 class="text-2xl font-medium">All Events</h2>
    <!-- render cad 4 times -->
    <section class="grid grid-cols-2 gap-8">
      <!-- use attribute binding ie :key to fetch the events -->
      <template v-if="!eventsLoading">
        <EventCard
          v-for="event in events"
          :key="event.id"
          :title="event.title"
          :when="event.date"
          :description="event.description"
          @register="console.log('Fine ho stay')"
        />
      </template>
      <template v-else>
      <LoadingEventCard v-for="i in 4" :key="i" />
      </template>
    </section>
    <!-- <section v-else>Loading events...</section> -->
    <h2 class="text-2xl font-medium">Your bookings</h2>
    <section class="grid grid-cols-1 gap-4">
      <BookingItem v-for="i in 3" :key="i" />
    </section>
  </main>
</template>

<script setup>
import { ref, onMounted } from "vue";
import BookingItem from "./components/BookingItem.vue";
import EventCard from "./components/EventCard.vue";
import LoadingEventCard from './components/LoadingEventCard.vue'

// define a ref that will hold the events
const events = ref([]);
// flag to indicate data loading
const eventsLoading = ref(false);

// function to fetch the data
const fetchEvents = async () => {
  eventsLoading.value = true;
  try {
    const response = await fetch("http://localhost:3001/events");
    events.value = await response.json();
  } finally {
    eventsLoading.value = false;
  }

  // console.log(events.value);
};

// initialize data fetching
onMounted(() => fetchEvents());
</script>
