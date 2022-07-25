<template>
  <div class="mt-5">
    <table class="table table-striped">
      <thead>
        <tr>
          <th>#</th>
          <th>Steps</th>
          <th>Cuts</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in listContainer" :key="item.id">
          <td>{{ index + 1 }}</td>
          <td>
            <input type="number" min="0" step="10" :value="item.steps"
              @input="event => updateItem('steps', item.id, event.target.value)">
          </td>
          <td>
            <input type="number" min="0" step="1" :value="item.cuts"
              @input="event => updateItem('cuts', item.id, event.target.value)">
          </td>
          <td>
            <button type="button" class="btn btn-primary btn-sm" @click="fillItem(item.id, item.steps)" :disabled="sumSteps >= globalSteps">Fill steps</button>
            <button type="button" class="btn btn-danger ms-2 btn-sm" @click="deleteItem(item.id)">Delete</button>
          </td>
        </tr>
        <tr>
          <td colspan="4">
            <button type="button" class="btn btn-primary" @click="addItem(0)" :disabled="listContainer.length >= 20">
              {{ listContainer.length >= 20 ? 'maximum of 20 reached' : 'Add' }}</button>
            <button type="button" class="btn btn-primary ms-2" @click="fillUp" :disabled="sumSteps >= globalSteps">Add item
              to fill to {{ globalSteps }} steps</button>
          </td>
        </tr>
      </tbody>
    </table>
    <p>{{ sumSteps }} steps / {{ globalSteps }} max <span class="badge bg-danger" v-show="sumSteps > globalSteps">too
        many steps</span></p>
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

const fillItem = (id: number, steps: number) => {
  const val: number = (Number(props.globalSteps) - Number(sumSteps.value)) + steps
  
  updateItem('steps', id, val)
}
</script>
