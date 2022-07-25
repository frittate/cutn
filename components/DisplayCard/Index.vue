<template>
  <div class="card mt-5 border shadow-sm" @click="copyToClipboard" :class="success ? ' border-success' : 'border-light'">
    <div class="card-body">
      <p>Code <span class="text-muted"> {{ success ? 'Copied!' : 'Click to copy to clipboard'}}</span></p>
      <pre ><slot /></pre>
    </div>
  </div>
</template>

<script setup>
import { useSlots, ref } from 'vue';

const slots = useSlots();
let success = ref(false);

const copyToClipboard = () => {
  const text = slots.default()[0].children

  navigator.clipboard.writeText(text).then(function () {
    success.value = true

    const timeOut = setTimeout(() => {
      success.value = false
    }, 2000)

    console.log('Async: Copying to clipboard was successful!');
  }, function (err) {
    console.error('Async: Could not copy text: ', err);
  });
}
</script>
