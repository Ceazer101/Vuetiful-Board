<template>
    <v-container class="background-container">
      <v-row>
        <v-col v-for="(column, index) in columns" :key="index" class="kanban-column">
          <v-card class="kanban-card">
            <v-card-title>{{ column.title }}</v-card-title>
            <v-divider></v-divider>
            <v-list dense>
              <v-list-item
                v-for="(task, taskIndex) in column.tasks"
                :key="taskIndex"
                class="kanban-task"
                draggable="true"
                @dragstart="onDragStart(task, column)"
                @dragover.prevent
                @drop="onDrop(column)"
              >
                <v-list-item-content>{{ task }}</v-list-item-content>
              </v-list-item>
            </v-list>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </template>
  
  <script lang="ts">
    import { defineComponent } from 'vue';
    import '../styles/KanbanBoard.scss';
  
  export default defineComponent({
    name: 'KanbanBoard',
    data() {
      return {
        columns: [
          {
            title: 'To Do',
            tasks: ['Task 1', 'Task 2', 'Task 3'],
          },
          {
            title: 'Doing',
            tasks: ['Task 4'],
          },
          {
            title: 'Done',
            tasks: ['Task 5'],
          },
        ],
        draggedTask: null as string | null,
        draggedFrom: null as { title: string; tasks: string[] } | null,
      };
    },
    methods: {
      onDragStart(task: string, column: { title: string; tasks: string[] }) {
        this.draggedTask = task;
        this.draggedFrom = column;
      },
      onDrop(column: { title: string; tasks: string[] }) {
        if (this.draggedTask && this.draggedFrom) {
          const taskIndex = this.draggedFrom.tasks.indexOf(this.draggedTask);
          if (taskIndex !== -1) this.draggedFrom.tasks.splice(taskIndex, 1);
          column.tasks.push(this.draggedTask);
          this.draggedTask = null;
          this.draggedFrom = null;
        }
      },
    },
  });
  </script>
  