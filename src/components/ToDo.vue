<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <!-- <form @submit.prevent="addtask"> -->
    <div class="no-task" v-if="taskListSize === 0">There is no task.</div>
    <div class="tasks" v-else>
      <table class="completed-tasks">
        <h1>completed</h1>
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
        <h1>uncompleted</h1>
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

  props: {
    msg: String,
  },
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
      var today = new Date();
      var dd = String(today.getDate()).padStart(2, "0");
      var mm = String(today.getMonth() + 1).padStart(2, "0");
      var yyyy = today.getFullYear();

      today = dd + "/" + mm + "/" + yyyy;
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
      if (taskItem.status == true) {
        taskItem.status = false;
      } else {
        taskItem.status = true;
      }
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


baran   murat   ali kağan
yasin   batu    ömer
        taner    