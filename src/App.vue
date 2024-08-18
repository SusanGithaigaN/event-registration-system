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
          :status="booking.status"
          @cancelled="cancelBooking(booking.id)"
        />
      </template>
      <template v-else>
        <LoadingBookingCard v-for="i in 4" :key="i" />
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

// find booking by ID
const findBookingById = (id) => bookings.value.findIndex((b) => b.id === id);

// create a new booking//POST
const handleRegistration = async (event) => {
  // prevent double booking by checking if booking exists
  // some(): check if array contains specific element
  // ^ returns true if at least one element satisfies a given condition
  if (
    bookings.value.some((booking) => booking.eventId === event.id && booking.userId === 1)
  ) {
    alert("You have already registered for this event");
    return;
  }

  const newBooking = {
    id: Date.now().toString(),
    userId: 1,
    eventId: event.id,
    eventTitle: event.title,
  };

  // add new booking object to the booking list
  bookings.value.push(newBooking);

  // check if POST request is successful & handle errors^^ change status
  try {
    // store data inside bookings obj
    // get res from fetch()
    const response = await fetch("http://localhost:3001/bookings", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        ...newBooking,
        status: "confirmed",
      }),
    });
    // check res from server
    // if ok, find booking.id
    // loop through booking[] to find id of new booking
    if (response.ok) {
      const index = findBookingById(newBooking.id);
      bookings.value[index] = await response.json();
    } else {
      throw new Error("Booking failed");
    }
  } catch (e) {
    // handle error & remove booking(b) from list
    console.error(`Failed to book event: `, e);
    bookings.value = bookings.value.filter((b) => b.id !== newBooking.id);
  }
};

// DELETE booking
const cancelBooking = async (bookingId) => {
  const index = findBookingById(bookingId);
  const originalBooking = bookings.value[index];
  bookings.value.splice(index, 1);

  try {
    const response = await fetch(`http://127.0.0.1:3001/bookings/ ${bookingId}`, {
      method: "DELETE",
    });
    if (!response.ok) {
      throw new Error("Unable to cancel booking");
    }
  } catch (e) {
    // if booking fails, remove 0 booking & return original bookings
    console.error("Failed to candel booking:", e);
    bookings.value.splice(index, 0, originalBooking);
  }
};

// initialize data fetching
onMounted(() => {
  fetchEvents();
  fetchBookings();
});
</script>
