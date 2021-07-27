<template>
  <b-container class="hello">
    <h1>Welcome to the ToDo App implemented with Vue</h1>
    <div class="no-task" v-if="taskListSize === 0">There is no task.</div>
    <div class="tasks" v-else>
      <h3>Completed Tasks</h3>
      <table class="completed-tasks">
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
          <tr v-for="taskItem in completedTasks" :key="taskItem.id">
            <td>
              <b-dropdown-item-button
                type="checkbox"
                default="unchecked"
                @click="changeStatus(taskItem)"
                ><b-icon icon="x-circle"></b-icon
              ></b-dropdown-item-button>
            </td>
            <td id="completed">{{ taskItem.title }}</td>
            <td id="completed">{{ taskItem.description }}</td>
            <td id="completed">{{ taskItem.dateCreated }}</td>
            <td>{{ taskItem.completedTime }}</td>
            <b-dropdown-item-button
              variant="danger"
              @click="deleteTask(taskItem)"
            >
              <b-icon icon="trash-fill" aria-hidden="true"></b-icon>
            </b-dropdown-item-button>
          </tr>
        </tbody>
      </table>
      <h3>Uncompleted Tasks</h3>
      <table class="tasks">
        <thead>
          <tr>
            <td></td>
            <td>Title</td>
            <td>Description</td>
            <td>Created Date</td>
            <td>Due Date</td>
          </tr>
        </thead>
        <tbody>
          <tr v-for="taskItem in unCompletedTasks" :key="taskItem.id">
            <td>
              <b-dropdown-item-button
                type="checkbox"
                default="unchecked"
                @click="changeStatus(taskItem)"
                ><b-icon icon="calendar2-check"></b-icon
              ></b-dropdown-item-button>
            </td>
            <b-td>{{ taskItem.title }}</b-td>
            <b-td>{{ taskItem.description }}</b-td>
            <b-td>{{ taskItem.dateCreated }}</b-td>
            <b-td>{{ getStringFormatOfDate(taskItem.dueDate) }}</b-td>
            <b-td>
              <b-dropdown-item-button
                variant="danger"
                @click="deleteTask(taskItem)"
              >
                <b-icon icon="trash-fill" aria-hidden="true"></b-icon>
              </b-dropdown-item-button>
              <b-dropdown-item-button
                v-b-modal.modal-scoped
                @click="setModalTask(taskItem)"
                ><b-icon
                  variant="warning"
                  icon="pencil-fill"
                  aria-hidden="true"
                ></b-icon>
              </b-dropdown-item-button>
            </b-td>
            <b-td v-if="isExpired(taskItem.dueDate)">
              <b-icon icon="exclamation-circle"></b-icon>
              This task is expired, you can edit it.
            </b-td>
            <b-td></b-td>
          </tr>
        </tbody>
      </table>
      <b-modal
        id="modal-scoped"
        :title="task.title"
      >
        <input type="text" placeholder="Title" v-model="tempTitle" />
        <input
          type="text"
          placeholder="Description"
          v-model="tempDescription"
        />
        <input type="datetime-local" v-model="date" />
        <template #modal-footer="{ok}"> 
          <b-button :disabled="!canAddTask" size="sm" variant="success" @click="editTask(task.id,ok)">
            <b-icon
            icon="pencil-fill"
            aria-hidden="true"
          ></b-icon
          >Update
          </b-button>
        </template>
      </b-modal>
    </div>
    <input type="text" placeholder="Title" v-model="tempTitle" />
    <input type="text" placeholder="description" v-model="tempDescription" />
    <input type="datetime-local" placeholder="Due Date" v-model="date" />
    <b-button
      type="submit"
      @click="addtask"
      :disabled="!canAddTask"
      variant="success"
      >Add task</b-button
    >
    <!-- </form> -->
  </b-container>
</template>

<script>
export default {
  name: "ToDo",

  data() {
    return {
      taskList: [],
      tempTitle: "",
      tempDescription: "",
      message: "",
      date: "",
      task: {
        id: null,
        title: null,
      },
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
    canAddTask() {
      return this.tempTitle != "" && this.tempDescription != "";
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
    getStringFormatOfDate(date) {
      var day = date.substring(8, 10);
      var month = date.substring(5, 7);
      var year = date.substring(0, 4);
      var hour = date.substring(11, 13);
      var minute = date.substring(14, 16);

      var arrangedDate =
        day + "/" + month + "/" + year + " " + hour + ":" + minute;
      return arrangedDate;
    },
    isExpired(dueDate) {
      var tzoffset = new Date().getTimezoneOffset() * 60000;
      var localISOTime = new Date(Date.now() - tzoffset)
        .toISOString()
        .slice(0, -1);
      return dueDate < localISOTime;
    },
    makeTaskItem() {
      const task = {
        id: this.taskListSize,
        completedTime: "",
        title: this.tempTitle,
        description: this.tempDescription,
        dateCreated: this.getDate(),
        status: false,
        dueDate: this.date,
      };
      return task;
    },
    addtask() {
      this.taskList.push(this.makeTaskItem());
      this.message = this.tempTitle + " added successfully!";
      this.makeToast(this.message, "success");
      this.message = "";
      this.tempTitle = "";
      this.tempDescription = "";
      this.date = "";
    },
    changeStatus(taskItem) {
      taskItem.completedTime = this.getDate();
      taskItem.status = !taskItem.status;
      this.message =
        "Status of the " +
        taskItem.title +
        " task has been changed successfully!";
      this.makeToast(this.message, "warning");
    },
    deleteTask(taskItem) {
      this.message = taskItem.title + " deleted successfully!";
      this.makeToast(this.message, "error");
      this.$set(
        this,
        "taskList",
        this.taskList.filter((element) => element.id != taskItem.id)
      );
    },
    editTask(editedID,ok) {
      console.log(editedID);
      let index = this.taskList.findIndex((element) => element.id == editedID);
      this.taskList[index].title = this.tempTitle;
      this.taskList[index].description = this.tempDescription;
      this.taskList[index].dueDate = this.date;
      this.tempTitle = "";
      this.tempDescription = "";
      ok();
    },
    makeToast(message, type) {
      this.$toast.open({
        message: message,
        type: type,
      });
    },
    setModalTask(task) {
      this.task.id = task.id;
      this.task.title = task.title;
    },
  },
  watch: {
    taskList: {
      deep: true,
      handler() {
        localStorage.setItem("taskList", JSON.stringify(this.taskList));
      },
    },
  },
  mounted() {
    const items = localStorage.getItem("taskList");
    this.taskList = JSON.parse(items ? items : "[]");
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
#completed {
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
