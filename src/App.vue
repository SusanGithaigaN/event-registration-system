<template>
  <main class="container mx-auto my-8 space-y-8">
    <h1 class="text-4xl font-medium">Event Booking</h1>
    <h2 class="text-2xl font-medium">All Events</h2>
    <!-- render cad 4 times -->
    <section class="grid grid-cols-2 gap-8">
      <EventCard
        v-for="i in 8"
        :key="i"
        title="Susan's book club meeting"
        when="12th August 2024"
        description="Bitch, I have no clue"
        @register="console.log('Fine ho stay')"
      />
    </section>
    <h2 class="text-2xl font-medium">Your bookings</h2>
    <section class="grid grid-cols-1 gap-4">
      <BookingItem v-for="i in 3" :key="i" />
    </section>
  </main>
</template>

<script setup>
import BookingItem from "./components/BookingItem.vue";
import EventCard from "./components/EventCard.vue";
</script>
