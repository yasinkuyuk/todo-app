<template>
  <b-container>
      <h3>Completed Tasks</h3>
    <table class="table table-hover completed-tasks">
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
  </b-container>
</template>

<script>
import {eventBus} from "../event"

export default{
    name: "CompletedTasks",
    props:{
        completedTasks:{
            type:Array,
            default:() => []
        }
    },
    data(){
        return{
            message:""
        }
    },
    methods:{
        changeStatus(task){
            this.message =
                "Status of the " +
                task.title +
                " task has been changed successfully!";
            eventBus.$emit("changeStatus",task.id);
            eventBus.$emit("toastMessage", this.message,"warning");
            this.message=""
        },
        deleteTask(task) {
            this.message = task.title + " deleted successfully!";
            eventBus.$emit("removeTask",task.id);
            eventBus.$emit("toastMessage",this.message,"error");
            this.message = "";
        }
    }
}

</script>

<style scoped>
#completed {
  text-decoration: line-through;
  color: gray;
}
</style>