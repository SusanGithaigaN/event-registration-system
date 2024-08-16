<template>
  <SectionCard>
    <template #header>
      {{ title }}
    </template>
    {{ when }}
    <template #footer>
      <div class="space-y-4">
        <p>{{ description }}</p>

        <div class="flex justify-end">
          <!-- handle button emit event -->
          <RoundButton @click="$emit('register')"> Register </RoundButton>
        </div>
      </div>
    </template>
  </SectionCard>
</template>

<script setup>
// import { title } from 'process';
import RoundButton from "./RoundButton.vue";
// import RoundedCard from "./RoundedCard.vue";
import SectionCard from "./SectionCard.vue";
// import Button from './RoundButton.vue';

defineProps({
  title: String,
  when: String,
  description: String,
});

defineEmits(["register"]);
</script>
