<template>
  <SectionCard>
    <div class="flex justify-between">
      {{ title }}
      <RoundButton variant="outline">Cancel </RoundButton>
    </div>
  </SectionCard>
</template>

<script setup>
// import RoundedCard from "./RoundedCard.vue";
import RoundButton from "./RoundButton.vue";
import SectionCard from "./SectionCard.vue";

defineProps({
  title: String
});
</script>
