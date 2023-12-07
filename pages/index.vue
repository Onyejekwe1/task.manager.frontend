<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <v-card>
          <v-card-title>Tasks</v-card-title>
          <v-divider></v-divider>
          <v-list>
            <v-list-item-group>
              <v-list-item v-for="task in tasks" :key="task.id">
                <v-list-item-content>
                  <v-list-item-title>{{ task.title }}</v-list-item-title>
                  <v-list-item-subtitle>{{ task.details }}</v-list-item-subtitle>
                </v-list-item-content>
                <v-list-item-action>
                  <v-btn color="blue" text @click="showTask(task)">View</v-btn>
                  <v-btn color="green" text @click="markComplete(task.id)">Complete</v-btn>
                </v-list-item-action>
              </v-list-item>
            </v-list-item-group>
          </v-list>
        </v-card>
      </v-col>
    </v-row>

    <!-- Task Detail Dialog -->
    <v-dialog v-model="dialog" max-width="600px">
      <v-card>
        <v-card-title>Task Detail</v-card-title>
        <v-card-text>
          <div><strong>Title:</strong> {{ selectedTask.title }}</div>
          <div><strong>Details:</strong> {{ selectedTask.details }}</div>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="red darken-1" text @click="dialog = false">Close</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      tasks: [],
      dialog: false,
      selectedTask: {},
    };
  },
  async mounted() {
    try {
      const response = await this.$axios.get('/api/tasks');
      console.log("Response data:", response.data); // Debugging line
      this.tasks = response.data;
      console.log("Tasks:", this.tasks); // Debugging line
    } catch (error) {
      console.error('Error fetching tasks:', error);
    }
  },
  methods: {
    showTask(task) {
      this.selectedTask = task;
      this.dialog = true;
    },
    async markComplete(id) {
      try {
        await this.$axios.put(`/api/tasks/${id}`, { completed: true });
        // Optionally, refresh the list or update the UI
      } catch (error) {
        console.error('Error marking task as complete:', error);
      }
    },
  },
};
</script>
