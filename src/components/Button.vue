
<template>
    <button class="btn" :class="classes" :disabled="disabled">
        <span v-if="loading" class="wheel-loadingIndicator"></span>
        <slot />
    </button>
</template>

<script setup>
import { ref } from 'vue'

defineProps({
  msg: String,
})
</script>