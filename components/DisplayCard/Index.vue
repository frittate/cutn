<template>
  <div class="card mt-5" @click="copyToClipboard">
    <div class="card-body">
      <p>Code:</p>
      <pre><slot /></pre>
    </div>
  </div>
</template>

<script setup>

import { useSlots } from 'vue';

const slots = useSlots();


const copyToClipboard = () => {
const text = slots.default()[0].children

console.log(slots.default());

  navigator.clipboard.writeText(text).then(function () {
    console.log('Async: Copying to clipboard was successful!');
  }, function (err) {
    console.error('Async: Could not copy text: ', err);
  });
}
</script>