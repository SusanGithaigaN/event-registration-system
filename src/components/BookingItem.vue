<template>
  <SectionCard>
  <!-- display POST booking status -->
    <div class="flex justify-between">
      <div class="flex space-x-3">
      <div>{{ title }}</div>
      <div>{{ status }}</div>
      </div>
      <RoundButton variant="outline">Cancel </RoundButton>
    </div>
  </SectionCard>
</template>

<script setup>
// import RoundedCard from "./RoundedCard.vue";
import RoundButton from "./RoundButton.vue";
import SectionCard from "./SectionCard.vue";

defineProps({
  title: String,
  status: String
});
</script>
