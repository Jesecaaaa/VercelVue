<template>
  <v-app>
    <v-container>
      <v-row class="justify-center">
        <v-col cols="12" sm="8" md="6">
          <v-card class="pa-5">
            <v-card-title class="headline">To-Do List</v-card-title>

            <!-- Input for new task and category in the same row -->
            <v-row class="d-flex align-center">
              <!-- Task input -->
              <v-col cols="8">
                <v-text-field
                  v-model="newTask"
                  label="Add a new task"
                  outlined
                  @keyup.enter="addTask"
                ></v-text-field>
              </v-col>

              <!-- Select category -->
              <v-col cols="4">
                <v-select
                  v-model="newCategory"
                  :items="categories"
                  label="Category"
                  outlined
                ></v-select>
              </v-col>
            </v-row>

            <v-btn color="primary" @click="addTask" :disabled="!newTask || !newCategory">
              Add Task
            </v-btn>

            <!-- List of tasks -->
            <v-list two-line>
              <v-list-item-group>
                <v-list-item v-for="(task, index) in tasks" :key="index">
                  <v-list-item-content>
                    <div class="d-flex justify-space-between align-center w-100">
                      <!-- Edit mode: Show input field for editing -->
                      <v-text-field
                        v-if="task.isEditing"
                        v-model="task.text"
                        outlined
                        dense
                        label="Edit task"
                        @keyup.enter="saveTask(index)"
                      ></v-text-field>

                      <!-- Display task text and details if not in edit mode -->
                      <v-list-item-title v-else>
                        {{ task.text }} <br />
                        <small>Added on: {{ formatDate(task.date) }}</small><br />
                        <small>Category: {{ task.category }}</small>
                      </v-list-item-title>

                      <!-- Edit and delete buttons -->
                      <div>
                        <v-btn icon color="green" @click="toggleEditTask(index)">
                          <v-icon v-if="!task.isEditing">mdi-pencil</v-icon>
                          <v-icon v-else>mdi-check</v-icon>
                        </v-btn>
                        <v-btn icon color="red" @click="deleteTask(index)">
                          <v-icon>mdi-delete</v-icon>
                        </v-btn>
                      </div>
                    </div>
                  </v-list-item-content>
                </v-list-item>
              </v-list-item-group>
            </v-list>

            <!-- Show message if no tasks -->
            <v-alert
              v-if="tasks.length === 0"
              type="info"
              class="mt-3"
            >
              No tasks yet. Add a task to get started!
            </v-alert>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      newCategory: "",
      tasks: [],
      categories: ["Important", "Urgent", "Urgent and Important"],
    };
  },
  methods: {
    // Add a new task with date and category
    addTask() {
      if (this.newTask && this.newCategory) {
        const now = new Date();
        this.tasks.push({
          text: this.newTask,
          category: this.newCategory,
          date: now,
          isEditing: false,
        });
        this.newTask = "";
        this.newCategory = "";
      }
    },
    // Delete a task from the list
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    // Toggle edit mode for a task
    toggleEditTask(index) {
      this.tasks[index].isEditing = !this.tasks[index].isEditing;
    },
    // Save the edited task
    saveTask(index) {
      this.tasks[index].isEditing = false;
    },
    // Format date to a readable string
    formatDate(date) {
      if (!date) return "";
      return new Intl.DateTimeFormat("en-US", {
        year: "numeric",
        month: "short",
        day: "numeric",
        hour: "numeric",
        minute: "numeric",
      }).format(new Date(date));
    },
  },
};
</script>

<style>
body {
  margin: 0;
  font-family: "Roboto", sans-serif;
}

.v-application {
  background-color: #f5f5f5;
}
</style>
