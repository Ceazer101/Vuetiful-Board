<template>
    <v-container class="background-container">
      <v-row>
        <v-col
            v-for="(column, index) in columns"
            :key="index"
            class="kanban-column"
            @dragover.prevent
            @drop="onDrop(column, null)"
        >
        <v-card class="kanban-card">
          <v-card-title>{{ column.title }}</v-card-title>
          <v-divider></v-divider>
          <v-list dense>
            <v-list-item
              v-for="(task, taskIndex) in column.tasks"
              :key="taskIndex"
              class="kanban-task"
              draggable="true"
              @dragstart="onDragStart(task, column, taskIndex)"
              @dragover.prevent
              @drop="onDrop(column, taskIndex)"
            >
              <v-list-item-content>
                <v-list-item-title>{{ task.title }}</v-list-item-title>
                <v-list-item-subtitle>{{ task.description }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
  
<script lang="ts">
import { defineComponent, reactive } from 'vue';
import '../styles/KanbanBoard.scss';
  
export default defineComponent({
  name: 'KanbanBoard',
  setup() {
    const state = reactive({
      columns: [
        {
          title: 'To Do',
          tasks: [
            { title: 'Task 1', description: 'This is the first task.' },
            { title: 'Task 2', description: 'This is the second task.' },
            { title: 'Task 3', description: 'This is the third task.' },
          ],
        },
        {
          title: 'Doing',
          tasks: [{ title: 'Task 4', description: 'This task is in progress.' }],
        },
        {
          title: 'Done',
          tasks: [{ title: 'Task 5', description: 'This task is completed.' }],
        },
      ],
      draggedTask: null as { title: string; description: string } | null,
      draggedFrom: null as { title: string; tasks: { title: string; description: string }[] } | null,
      draggedTaskIndex: null as number | null,
    });
  
    // Methods
    const onDragStart = (
      task: { title: string; description: string }, 
      column: { title: string; tasks: { title: string; description: string }[] }, 
      taskIndex: number
    ) => {
      state.draggedTask = task;
      state.draggedFrom = column;
      state.draggedTaskIndex = taskIndex;
    };

    const onDrop = (
      column: { title: string; tasks: { title: string; description: string }[] }, 
      taskIndex: number | null
    ) => {
      if (state.draggedTask && state.draggedFrom) {
        const fromTaskIndex = state.draggedFrom.tasks.indexOf(state.draggedTask);
        if (fromTaskIndex !== -1) state.draggedFrom.tasks.splice(fromTaskIndex, 1);

        if (taskIndex !== null) {
        column.tasks.splice(taskIndex, 0, state.draggedTask);
        } else {
        column.tasks.push(state.draggedTask);
        }

        state.draggedTask = null;
        state.draggedFrom = null;
        state.draggedTaskIndex = null;
      }
    };
    return { ...state, onDragStart, onDrop };
  }
});
</script>
  