<template>
  <v-card-title class="text-h5 font-weight-bold">
    Task Manager
  </v-card-title>

  <v-card-text>
    <!-- Task Input -->
    <v-text-field
      v-model="newTask"
      label="Add new task"
      append-inner-icon="mdi-plus"
      @click:append-inner="addTask"
      @keyup.enter="addTask"
      variant="outlined"
      density="comfortable"
      :rules="rules"
      ref="taskInput"
    ></v-text-field>

    <!-- Task List -->
    <v-list class="task-list">
      <draggable 
        v-model="tasks"
        item-key="id"
        handle=".handle"
        @end="updateTaskOrder"
        :ghost-class="'ghost'"
      >
        <template #item="{element: task, index}">
          <div>
            <v-divider v-if="index !== 0"></v-divider>
            <v-list-item>
              <template v-slot:prepend>
                <v-icon class="handle mr-2" color="grey">mdi-drag</v-icon>
                <v-checkbox
                  v-model="task.completed"
                  @change="updateTaskOrder"
                  hide-details
                ></v-checkbox>
              </template>
              
              <v-list-item-title
                :class="{ 'text-decoration-line-through': task.completed }"
              >
                {{ task.text }}
              </v-list-item-title>

              <template v-slot:append>
                <v-btn
                  icon="mdi-delete"
                  size="small"
                  color="error"
                  variant="text"
                  @click="removeTask(task.id)"
                ></v-btn>
              </template>
            </v-list-item>
          </div>
        </template>
      </draggable>
    </v-list>
  </v-card-text>
</template>

<script setup>
import { ref } from 'vue'
import draggable from 'vuedraggable'  // Changed this line

const newTask = ref('')
const taskInput = ref(null)
const tasks = ref([])
let nextId = 0

const rules = [(v) => !!v || 'Task cannot be empty']

const addTask = () => {
  if (newTask.value.trim()) {
    tasks.value.push({
      id: nextId++,
      text: newTask.value.trim(),
      completed: false
    })
    newTask.value = ''
    taskInput.value.reset()
  }
}

const removeTask = (taskId) => {
  const index = tasks.value.findIndex(task => task.id === taskId)
  if (index !== -1) {
    tasks.value.splice(index, 1)
  }
}

const updateTaskOrder = () => {
  // No need to do anything here anymore as the v-model handles the updates
}
</script>

<style scoped>
.v-list-item {
  min-height: 44px;
}

.handle {
  cursor: move;
  cursor: -webkit-grabbing;
}

.task-list {
  min-height: 50px;
}

.ghost {
  opacity: 0;
}
</style>