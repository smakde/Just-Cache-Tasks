<template>
  <v-container class="mt-6">
    <v-row justify="space-between" align="center">
      <v-col>
        <h1 class="text-h3 font-weight-bold text-primary mb-4">
          <!-- <v-icon start class="mr-2">mdi-calendar-check</v-icon> -->
          To-do List
        </h1>
        <h5>Your own offline to do list!</h5>
        <h5>Save this to your phone as an App!</h5>
        <br />
      </v-col>
    </v-row>

    <!-- Add Tasks Form -->
    <v-form @submit.prevent="addTask">
      <v-text-field
        v-model="newTask"
        label="New Tasks"
        outlined
        dense
        class="mb-4"
      />
      <v-btn type="submit" color="primary">Add</v-btn>
    </v-form>

    <!-- Tasks List -->
    <v-row class="mt-6" dense>
      <v-col
        v-for="(task, index) in tasks"
        :key="index"
        cols="12"
        sm="6"
        md="4"
      >
        <v-card>
          <v-card-title>
            <v-checkbox
              v-model="task.completed"
              :label="task.name"
              @change="updateStorage"
            />
            <p
              @click="deleteTask(index)"
              style="
                cursor: pointer;
                color: red;
                position: relative;
                left: 10px;
              "
            >
              Remove
            </p>
          </v-card-title>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "DashboardView",

  data() {
    return {
      newTask: "",
      tasks: [],
    };
  },

  computed: {
    isDark() {
      return this.theme.global.name.value === "dark";
    },
  },

  created() {
    const saved = localStorage.getItem("tasks");
    this.tasks = saved ? JSON.parse(saved) : [];
  },

  methods: {
    addTask() {
      if (!this.newTask.trim()) return;
      this.tasks.push({ name: this.newTask.trim(), completed: false });
      this.newTask = "";
      this.updateStorage();
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
      this.updateStorage();
    },
    updateStorage() {
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
  },
};
</script>
