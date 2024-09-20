<template>
  <v-container>
    <!-- Title -->
    <v-row justify="center">
      <v-col cols="12" md="8">
        <v-toolbar flat color="transparent">
          <v-icon large color="primary">mdi-checkbox-marked-circle-outline</v-icon>
          <v-toolbar-title class="ml-2">
            <span class="title font-weight-bold blue--text text--darken-3">My Todo-s</span>
          </v-toolbar-title>
        </v-toolbar>
      </v-col>
    </v-row>

    <!-- Add Task -->
    <v-row justify="center">
      <v-col cols="12" md="8">
        <v-card>
          <v-card-text>
            <v-row>
              <v-col cols="12" sm="10">
                <v-text-field
                  v-model="newTask.title"
                  label="Add new..."
                  solo
                  hide-details
                  dense
                ></v-text-field>
              </v-col>
              <v-col cols="12" sm="2">
                <v-btn color="primary" block @click="addTask">
                  ADD
                </v-btn>
              </v-col>
            </v-row>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>

    <!-- Task List as Table -->
    <v-row justify="center">
      <v-col cols="12" md="8">
        <v-data-table
          :headers="headers"
          :items="tasks"
          item-key="title"
          class="elevation-1"
          dense
        >
          <template v-slot:item.completed="{ item }">
            <v-checkbox v-model="item.completed"></v-checkbox>
          </template>

          <template v-slot:item.title="{ item }">
            <v-text-field
              v-if="item.isEditing"
              v-model="item.title"
              hide-details
              dense
              solo
            ></v-text-field>
            <span v-else>{{ item.title }}</span>
          </template>

          <template v-slot:item.actions="{ item, index }">
            <v-btn icon @click="toggleEditTask(item)">
              <v-icon v-if="item.isEditing" color="green">mdi-check</v-icon>
              <v-icon v-else color="blue">mdi-pencil</v-icon>
            </v-btn>
            <v-btn icon @click="removeTask(index)">
              <v-icon color="red">mdi-delete</v-icon>
            </v-btn>
          </template>

          <template v-slot:no-data>
            <v-alert :value="true" color="info" icon="mdi-information">
              No tasks available. Add a new task to get started.
            </v-alert>
          </template>
        </v-data-table>
      </v-col>
    </v-row>

    <!-- Task Summary -->
    <v-row justify="center" class="mt-4">
      <v-col cols="12" md="8">
        <v-card class="pa-3">
          <v-row>
            <v-col cols="4">
              <div>Total Tasks: <strong>{{ totalTasks }}</strong></div>
            </v-col>
            <v-col cols="4">
              <div>Completed Tasks: <strong>{{ completedTasks }}</strong></div>
            </v-col>
            <v-col cols="4">
              <div>Incomplete Tasks: <strong>{{ incompleteTasks }}</strong></div>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      newTask: {
        title: '',
        completed: false,
      },
      tasks: [],
      headers: [
        { text: 'Task Title', value: 'title', align: 'start' },
        { text: 'Completed', value: 'completed', align: 'center', width: '100px' },
        { text: 'Actions', value: 'actions', align: 'center', sortable: false, width: '150px' },
      ],
    };
  },
  computed: {
    totalTasks() {
      return this.tasks.length;
    },
    completedTasks() {
      return this.tasks.filter(task => task.completed).length;
    },
    incompleteTasks() {
      return this.tasks.filter(task => !task.completed).length;
    },
  },
  methods: {
    addTask() {
      if (this.newTask.title) {
        this.tasks.push({
          ...this.newTask,
          isEditing: false,
        });
        this.newTask.title = '';
      }
    },
    toggleEditTask(task) {
      // Toggle the editing state and stop editing if task is done
      if (task.isEditing) {
        task.isEditing = false; // Save changes and stop editing
      } else {
        task.isEditing = true; // Start editing
      }
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    },
  },
};
</script>

<style scoped>
.title {
  font-size: 28px;
}

.v-card {
  background-color: #424242;
}

.v-list-item-title {
  font-weight: 500;
}

.v-checkbox input:checked + .v-icon {
  color: blue;
}

@media (max-width: 600px) {
  .title {
    font-size: 24px;
  }
  .v-toolbar-title {
    font-size: 20px;
  }
}
</style>
