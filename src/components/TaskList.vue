<template>
  <b-container>
    {{ taskListSize }}
    <div v-if="taskListSize === 0">There is no task!</div>
    <div v-else>
      <CompletedTasks :completedTasks="completedTasks" />
      <UncompletedTasks :unCompletedTasks="unCompletedTasks" />
    </div>
  </b-container>
</template>
<script>
import CompletedTasks from "./CompletedTasks.vue";
import UncompletedTasks from "./UncompletedTasks.vue";
import { eventBus } from "../event";

export default {
  name: "TaskList",
  components: {
    CompletedTasks,
    UncompletedTasks,
  },
  data() {
    return {
      taskList: [],
      id: 0,
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
    pushTaskToList(task) {
      task.id = this.id;
      this.id++;
      this.taskList.push(task);
    },
    getDate() {
      var date = new Date();
      var day = String(date.getDate()).padStart(2, "0");
      var month = String(date.getMonth() + 1).padStart(2, "0");
      var year = date.getFullYear();
      var hour = date.getHours();
      var minute = date.getMinutes();
      var second = date.getSeconds();

      var today =
        day +
        "/" +
        month +
        "/" +
        year +
        " " +
        hour +
        ":" +
        minute +
        ":" +
        second;
      return today;
    },
    changeStatusOfTask(taskID) {
      let index = this.taskList.findIndex((taskItem) => taskItem.id == taskID);
      this.taskList[index].status = !this.taskList[index].status;
      this.taskList[index].completedTime = this.getDate();
    },
    deleteTask(taskID) {
      this.$set(
        this,
        "taskList",
        this.taskList.filter((element) => element.id != taskID)
      );
    },
    editTaskInList(taskID, newTitle, newDescription, newDueDate) {
      let index = this.taskList.findIndex((element) => element.id == taskID);
      this.taskList[index].title = newTitle;
      this.taskList[index].description = newDescription;
      this.taskList[index].dueDate = newDueDate;
    },
  },
  watch: {
    taskList: {
      deep: true,
      handler() {
        localStorage.setItem("taskList", JSON.stringify(this.taskList));
      },
    },
    id: {
      deep: true,
      handler() {
        localStorage.setItem("id", JSON.stringify(this.id));
      },
    },
  },
  mounted() {
    const items = localStorage.getItem("taskList");
    this.taskList = JSON.parse(items ? items : "[]");
    this.id = localStorage.getItem("id") ? Number(localStorage.getItem("id")) : 0;
    eventBus.$on("addTaskToList", this.pushTaskToList);
    eventBus.$on("changeStatus", this.changeStatusOfTask);
    eventBus.$on("removeTask", this.deleteTask);
    eventBus.$on("editTask", this.editTaskInList);
  },
  beforeDestroy() {
    eventBus.$off("addTaskToList", this.pushTaskToList);
    eventBus.$off("changeStatus", this.changeStatusOfTask);
    eventBus.$off("removeTask", this.deleteTask);
    eventBus.$off("editTask", this.editTaskInList);
  },
};
</script>
