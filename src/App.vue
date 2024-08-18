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
          @register="handleRegistration(event)"
        />
      </template>
      <template v-else>
        <LoadingEventCard v-for="i in 4" :key="i" />
      </template>
    </section>
    <!-- <section v-else>Loading events...</section> -->
    <h2 class="text-2xl font-medium">Your bookings</h2>
    <section class="grid grid-cols-1 gap-4">
      <!-- <template v-if="!bookingsLoading"> -->
      <template v-if="!bookingsLoading">
        <BookingItem
          v-for="booking in bookings"
          :key="booking.id"
          :title="booking.eventTitle"
        />
      </template>
      <template v-else>
      <LoadingBookingCard v-for="i in 4 " :key="i" />
      </template>
    </section>
  </main>
</template>

<script setup>
import { ref, onMounted } from "vue";
import BookingItem from "./components/BookingItem.vue";
import EventCard from "./components/EventCard.vue";
import LoadingEventCard from "./components/LoadingEventCard.vue";
import LoadingBookingCard from "./components/LoadingBookingCard.vue";

// define a ref that will hold the events & bookings
const events = ref([]);
const bookings = ref([]);
// flag to indicate data loading
const eventsLoading = ref(false);
const bookingsLoading = ref(false);

// function to fetch the events
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

// fetch bookings
const fetchBookings = async () => {
  bookingsLoading.value = true;
  try {
    const response = await fetch("http://localhost:3001/bookings");
    bookings.value = await response.json();
  } finally {
    bookingsLoading.value = false;
  }
};

// create a new booking//POST
const handleRegistration = async (event) => {
  const newBooking = {
    id: Date.now().toString(),
    userId: 1,
    eventId: event.id,
    eventTitle: event.title,
  };

  // store data inside bookings obj
  await fetch("http://localhost:3001/bookings", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({
      ...newBooking,
      status: "confirmed",
    }),
  });
};

// initialize data fetching
onMounted(() => {
  fetchEvents();
  fetchBookings();
});
</script>
