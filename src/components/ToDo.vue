<template>
  <div class="hello">
    <h1>Welcome to the ToDo App implemented with Vue</h1>
    <div class="no-task" v-if="taskListSize === 0">There is no task.</div>
    <div class="tasks" v-else>
      <table class="completed-tasks">
        <h3>completed</h3>
        <tr v-for="taskItem in completedTasks" :key="taskItem.id" id="completed">
          <td>
            <input type="checkbox" v-model="taskItem.status" />
          </td>
          <td>{{ taskItem.title }}</td>
          <td>{{ taskItem.description }}</td>
          <td>{{ taskItem.dateCreated }}</td>

        </tr>
      </table>
      <table class="tasks">
        <h3>uncompleted</h3>
        <tr v-for="taskItem in unCompletedTasks" :key="taskItem.id">
          <td>
            <input type="checkbox" default="unchecked" v-model="taskItem.status" />
          </td>
          <td>{{ taskItem.title }}</td>
          <td>{{ taskItem.description }}</td>
          <td>{{ taskItem.dateCreated }}</td>
        </tr>
      </table>
      
    </div>
    <input type="text" placeholder="Title" v-model="tempTitle" />
    <input
      type="text"
      placeholder="description"
      v-model="tempDescription"
    />
    <button type="submit" @click="addtask">Add task</button>
    <!-- </form> -->
  </div>
</template>

<script>
export default {
  name: "ToDo",

  data() {
    return {
      taskList: [],
      tempTitle: "",
      tempDescription: "",
    };
  },
  computed: {
    taskListSize() {
      return this.taskList.length;
    },
    completedTasks() {
      return this.taskList.filter((t) => {
        return t.status == true;
      });
    },
    unCompletedTasks() {
      return this.taskList.filter((t) => {
        return t.status == false;
      });
    },
  },
  methods: {
    getDate() {
      var date = new Date();
      var day = String(date.getDate()).padStart(2, "0");
      var month = String(date.getMonth() + 1).padStart(2, "0");
      var year = date.getFullYear();

      var today = day + "/" + month + "/" + year;
      return today;
    },
    makeTaskItem() {
      const task = {
        id: "",
        title: this.tempTitle,
        description: this.tempDescription,
        dateCreated: this.getDate(),
        status: false,
      };
      return task;
    },
    addtask() {
      this.taskList.push(this.makeTaskItem());
      this.tempTitle = "";
      this.tempDescription = "";
    },
    changeStatusDone(taskItem) {
      taskItem.status = !taskItem.status;
    },
  },
};
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
#completed{
  text-decoration: line-through;
  color: gray;
}
</style>
