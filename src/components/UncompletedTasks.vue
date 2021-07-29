<template>
  <b-container>
    <h3>Uncompleted Tasks</h3>
    <table class="table table-hover">
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
  </b-container>
</template>

<script>
import {eventBus} from "../event"
export default {
  name: "UncompletedTasks",
  data() {
    return {
      message: "",
    };
  },
  methods: {
    changeStatus(task) {
      this.message =
        "Status of the " + task.title + " task has been changed successfully!";
      eventBus.$emit("changeStatus", task.id);
      eventBus.$emit("toastMessage", this.message, "warning");
      this.message = "";
    },
    deleteTask(task) {
      this.message = task.title + " deleted successfully!";
      eventBus.$emit("removeTask", task.id);
      eventBus.$emit("toastMessage", this.message, "error");
      this.message = "";
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
    setModalTask(taskItem){
        eventBus.$emit("setModalProperties",taskItem);
    }
  },
  props:{
      unCompletedTasks:{
          type: Array,
          default: ()=> []
      }
  }
};
</script>
