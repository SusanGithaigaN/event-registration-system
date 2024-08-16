<template>
  <RoundedCard>
  <!-- check if the header was supplied to the h3 tag -->
    <h3 v-if="$slots.header" class="p-4 font-medium text-xl border-b border-gray-200">
      <slot name="header"></slot>
    </h3>
    <!-- use class binding {}syntax to utilize classnames even when the slot is empty -->
    <div v-if="$slots.default" :class="{'p-4': true, 'border-b border-gray-200' : $slots.footer}">
      <slot></slot>
    </div>
    <div v-if="$slots.footer" class="p-4">
      <slot name="footer"></slot>
      <!-- use $slots.header to check if the specific template for a given slot was added
      <div v-if="$slots.header"></div> -->
    </div>
  </RoundedCard>
</template>

<script setup>
import RoundedCard from "./RoundedCard.vue";
</script>
