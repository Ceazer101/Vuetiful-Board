<template>
    <v-container class="background-container">
      <v-row>
        <v-col
            v-for="(column, index) in columns"
            :key="index"
            class="kanban-column"
            @dragover.prevent
            @drop="onDrop(column)"
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
                @dragstart="onDragStart(task, column)"
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
  import { defineComponent } from 'vue';
  import '../styles/KanbanBoard.scss';
  
  export default defineComponent({
    name: 'KanbanBoard',
    data() {
      return {
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
      };
    },
    methods: {
      onDragStart(task: { title: string; description: string }, column: { title: string; tasks: { title: string; description: string }[] }) {
        this.draggedTask = task;
        this.draggedFrom = column;
      },
      onDrop(column: { title: string; tasks: { title: string; description: string }[] }) {
        if (this.draggedTask && this.draggedFrom) {
            const taskIndex = this.draggedFrom.tasks.indexOf(this.draggedTask);
            if (taskIndex !== -1) this.draggedFrom.tasks.splice(taskIndex, 1);
            column.tasks.push(this.draggedTask);
            this.draggedTask = null;
            this.draggedFrom = null;
        }
      }
    }
  });
  </script>
  