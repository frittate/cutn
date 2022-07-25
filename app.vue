<template>
  <div>
    <div>
      <input type="number" name="" id="" v-model="globalSteps">
      <h2>cut_overview</h2>
      <ListView v-model="state.cut_overview_container" :global-steps="globalSteps" />
      <p>{{ displayOverview }}</p>
    </div>

    <div>
      <h2>cut_innercut</h2>
      <ListView v-model="state.cut_innercut_container" :global-steps="globalSteps" />
      <p>{{ displayInnercut }}</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ListItem } from '~/utils/interfaces';

const state = reactive({
  cut_overview_container: [{ id: 1, steps: 400, cuts: 12 }, { id: 2, steps: 600, cuts: 4 }],
  cut_innercut_container: [{ id: 1, steps: 400, cuts: 4 }, { id: 2, steps: 600, cuts: 12 }]
})

const globalSteps = ref(1000)

const displayOverview = computed(() => {
  return JSON.stringify(state.cut_overview_container)
})

const displayInnercut = computed(() => state.cut_innercut_container.map((item: ListItem) => `[${item.cuts}]*${item.steps}`).join('+')
)

</script>
