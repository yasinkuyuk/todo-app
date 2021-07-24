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
            <button type="checkbox" default="unchecked" @click="changeStatus(taskItem)"></button>
          </td>
          <td id="completed">{{ taskItem.title }}</td>
          <td id="completed">{{ taskItem.description }}</td>
          <td id="completed">{{ taskItem.dateCreated }}</td>
          <td>{{ taskItem.completedTime}}</td>
          <button @click="deleteTask(taskItem)">Delete Task</button>
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
              <button type="checkbox" default="unchecked" @click="changeStatus(taskItem)"></button>
            </td>
            <td>{{ taskItem.title }}</td>
            <td>{{ taskItem.description }}</td>
            <td>{{ taskItem.dateCreated }}</td>
            <button @click="deleteTask(taskItem)">Delete Task</button>
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
      index: 0,
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
        id: this.index,
        completedTime: "",
        title: this.tempTitle,
        description: this.tempDescription,
        dateCreated: this.getDate(),
        status: false,
      };
      return task;
    },
    addtask() {
      if(this.tempTitle != "" && this.tempDescription != ""){
        this.taskList.push(this.makeTaskItem());
        this.tempTitle = "";
        this.tempDescription = "";
        this.index += 1;
      }
      else {
        alert("Please enter non-null task");
      }
    },
    changeStatus(taskItem) {
      taskItem.completedTime = this.getDate();
      taskItem.status = !taskItem.status;
    },
    deleteTask(taskItem) {
      delete this.taskList[taskItem.id];
    }
  },
  watch: {
    taskList : {
      deep : true,
      handler(){
        localStorage.setItem("taskList", JSON.stringify(this.taskList));
        const index = String(this.index);
        localStorage.setItem("lastIndex",index);
      }
    }
  },
  mounted(){
    const items = localStorage.getItem("taskList");
    this.taskList = JSON.parse(items ? items : "[]");
    const lastIndex = localStorage.getItem("lastIndex");
    const index  = lastIndex ? lastIndex : 0 ;
    this.index = parseInt(index);
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

caption {
  font-weight: bold;
}

thead {
  font-weight: bold;
}

</style>
