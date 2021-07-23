<template>
  <div class="hello">
    <h1>Welcome to the ToDo App implemented with Vue</h1>
    <div class="no-task" v-if="taskListSize === 0">There is no task.</div>
    <div class="tasks" v-else>
      <table class="completed-tasks">
        <caption>Completed Tasks</caption>
        <thead>
          <tr>
            <td></td>
            <td>Title</td>
            <td>Description</td>
            <td>Created Date</td>
            <td>Completed Time</td>
          </tr>
        </thead>
        <tbody>
          <tr  v-for="taskItem in completedTasks" :key="taskItem.id">
          <td>
            <button type="checkbox" default="unchecked" @click="changeStatusDone(taskItem)"></button>
          </td>
          <td id="completed">{{ taskItem.title }}</td>
          <td id="completed">{{ taskItem.description }}</td>
          <td id="completed">{{ taskItem.dateCreated }}</td>
          <td>{{ taskItem.completedTime}}</td>
        </tr>
        </tbody>   
      </table>
      <table class="tasks">
        <caption>Uncompleted Tasks</caption>
        <thead>
          <tr>
            <td></td>
            <td>Title</td>
            <td>Description</td>
            <td>Created Date</td>
          </tr>
        </thead>
        <tbody>
          <tr v-for="taskItem in unCompletedTasks" :key="taskItem.id">
            <td>
              <button type="checkbox" default="unchecked" @click="changeStatusDone(taskItem)"></button>
            </td>
            <td>{{ taskItem.title }}</td>
            <td>{{ taskItem.description }}</td>
            <td>{{ taskItem.dateCreated }}</td>
          </tr>
        </tbody>
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
      var hour = date.getHours();
      var minute = date.getMinutes();
      var second = date.getSeconds();

      var today = day + "/" + month + "/" + year + " "+hour+":"+minute+":"+second;
      return today;
    },
    makeTaskItem() {
      const task = {
        completedTime: "",
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
      taskItem.completedTime = this.getDate();
      console.log("time: ",taskItem.completedTime);
      taskItem.status = !taskItem.status;
    },
  },
  watch: {
    taskList : {
      deep : true,
      handler(){
        const taskList = this.taskList;
        localStorage.setItem("taskList", JSON.stringify(taskList));
      }
    }
  },
  mounted(){
    const items = localStorage.getItem("taskList");
    const taskList = JSON.parse(items ? items : "[]")
    this.taskList = taskList;
  }
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

.completed-tasks {
  align-content: center;
}

caption {
  font-weight: bold;
}

thead {
  font-weight: bold;
}

</style>
