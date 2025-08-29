<template>
  <v-card class="mx-auto my-4" max-width="400">
    <v-card-title class="text-h6">
      My TODO List
    </v-card-title>

    <v-card-text>
      <v-list>
        <v-list-item v-for="(task, index) in tasks" :key="index">
          <v-checkbox
            v-model="task.completed"
            :label="task.text"
            :class="{ 'text-decoration-line-through': task.completed }"
          ></v-checkbox>
          <v-btn
            icon="mdi-close"
            size="small"
            color="error"
            @click="removeTask(index)"
          ></v-btn>
        </v-list-item>
      </v-list>

      <v-divider class="my-4"></v-divider>

      <v-text-field
        v-model="newTask"
        label="Add new task"
        append-inner-icon="mdi-plus"
        @click:append-inner="addTask"
        @keyup.enter="addTask"
        variant="outlined"
        density="comfortable"
      ></v-text-field>
    </v-card-text>
  </v-card>
</template>

<script setup>
import { ref } from 'vue'

const newTask = ref('')
const tasks = ref([
  { text: 'Learn Vue.js', completed: false },
  { text: 'Build a TODO app', completed: false },
  { text: 'Master Vuetify', completed: false }
])

const addTask = () => {
  if (newTask.value.trim()) {
    tasks.value.push({
      text: newTask.value,
      completed: false
    })
    newTask.value = ''
  }
}

const removeTask = (index) => {
  tasks.value.splice(index, 1)
}
</script>

<style scoped>
.text-decoration-line-through {
  text-decoration: line-through;
}
</style>