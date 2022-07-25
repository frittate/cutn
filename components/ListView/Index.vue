<template>
  <div>
    <table>
      <thead>
        <tr>
          <th>Cuts</th>
          <th>Steps</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in listContainer" :key="item.id">
          <td>
            <input type="number" :value="item.cuts" @input="event => updateItem('cuts', item.id, event.target.value)">
          </td>
          <td>
            <input type="number" :value="item.steps" @input="event => updateItem('steps', item.id, event.target.value)">
          </td>
          <td>
            <button @click="deleteItem(item.id)">Delete</button>
          </td>
        </tr>
        <tr>
          <td>
            <button @click="addItem(0)">Add</button>
            <button @click="fillUp" :disabled="sumSteps >= globalSteps">Fill to {{ globalSteps }}</button>
          </td>
        </tr>
      </tbody>
    </table>
    <p>{{ sumSteps }}</p>
  </div>
</template>

<script setup lang="ts">
import { ListItem } from '~/utils/interfaces';

const props = defineProps(['modelValue', 'globalSteps'])
// const listContainer = toRef(props, 'modelValue')

const listContainer = ref([]);
const listContainerRef = toRef(props, 'modelValue');

watch(listContainerRef, (value) => {
  listContainer.value = value;
}, { immediate: true });

const emit = defineEmits(['update:modelValue', 'remove'])

const sumSteps = computed(() => {
  return listContainer.value.map((item: ListItem) => item.steps).reduce((a: number, b: number) => a + b, 0)
})

const updateItem = (target: string, id: number, val: number) => {
  for (const item of listContainer.value) {
    if (item.id === id) {

      item[target] = Number(val)

      break;
    }
  }

  emit('update:modelValue', listContainer)
}

const addItem = (steps?: number) => {
  const ids = listContainer.value.map((item: ListItem) => {
    return item.id;
  });

  const maxId = Math.max(...ids);
  const newSteps = steps || 0
  const newItem: ListItem = { id: maxId + 1, steps: newSteps, cuts: 0 }
  listContainer.value.push(newItem)

  emit('update:modelValue', listContainer)
}

const deleteItem = (id: number) => {
  listContainer.value = listContainer.value.filter((item: ListItem) => item.id !== id)
  emit('update:modelValue', listContainer)
}

const fillUp = () => {
  const missingSteps = props.globalSteps - sumSteps.value
  addItem(missingSteps)
}
</script>
