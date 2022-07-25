<template>
  <NavBar />
  <main class="container">

    <h1>cutn schedule helper</h1>
    <div class="row">
      <h2>Settings</h2>
      <div class="mb-3">
        <label for="globalSteps" class="form-label">Global Steps</label>
        <input type="number" v-model="globalSteps" disabled class="form-control" id="globalSteps">
      </div>
    </div>
    <div class="row">
      <div class="col">
        <h2>cut_overview</h2>

        <Chart :input-data="state.cut_overview_container" :global-steps="globalSteps" data-label="cut_overview" />
        <DisplayCard>{{ displayOverview }}</DisplayCard>
        <ListView v-model="state.cut_overview_container" :global-steps="globalSteps" />
      </div>

      <div class="col">
        <h2>cut_innercut</h2>

        <Chart :input-data="state.cut_innercut_container" :global-steps="globalSteps" data-label="cut_innercut" />
        <DisplayCard>{{ displayInnercut }}</DisplayCard>
        <ListView v-model="state.cut_innercut_container" :global-steps="globalSteps" />

      </div>
    </div>

  </main>
</template>

<script setup lang="ts">
import { ListItem } from '~/utils/interfaces';
import NavBar from './components/NavBar/Index.vue';

const state = reactive({
  cut_overview_container: [{ id: 1, steps: 400, cuts: 12 }, { id: 2, steps: 600, cuts: 4 }],
  cut_innercut_container: [{ id: 1, steps: 400, cuts: 4 }, { id: 2, steps: 600, cuts: 12 }]
})

const globalSteps = ref(1000)

const displayOverview = computed(() => state.cut_overview_container.map((item: ListItem) => `[${item.cuts}]*${item.steps}`).join('+'))

const displayInnercut = computed(() => state.cut_innercut_container.map((item: ListItem) => `[${item.cuts}]*${item.steps}`).join('+'))

</script>
