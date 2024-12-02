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
        draggedTaskIndex: null as number | null,
      };
    },
    methods: {
        onDragStart(task: { title: string; description: string }, column: { title: string; tasks: { title: string; description: string }[] }, taskIndex: number) {
            this.draggedTask = task;
            this.draggedFrom = column;
            this.draggedTaskIndex = taskIndex;
        },
      onDrop(column: { title: string; tasks: { title: string; description: string }[] }, taskIndex: number | null) {
        if (this.draggedTask && this.draggedFrom) {
        const fromTaskIndex = this.draggedFrom.tasks.indexOf(this.draggedTask);
        if (fromTaskIndex !== -1) this.draggedFrom.tasks.splice(fromTaskIndex, 1);

        if (taskIndex !== null) {
          column.tasks.splice(taskIndex, 0, this.draggedTask);
        } else {
          column.tasks.push(this.draggedTask);
        }

        this.draggedTask = null;
        this.draggedFrom = null;
        this.draggedTaskIndex = null;
        }
      }
    }
  });
  </script>
  