<template>
  <div>
    <TaskList />
    <AddTask />
    <EditModal />
  </div>
</template>

<script>
//import ToDo from './components/ToDo.vue'
// import Ahmet from "./components/Ahmet.vue"
import TaskList from "./components/TaskList.vue"
import AddTask from "./components/AddTask.vue"
import EditModal from "./components/EditModal.vue"
import {eventBus} from "./event"

export default {
  name: 'App',
  components: {
    TaskList,
    AddTask,
    EditModal
  },
  data(){
    return{
    }
  },

  methods:{
    makeToast(message, type) {
      this.$toast.open({
        message: message,
        type: type,
      });
    }
  },
  mounted(){
    eventBus.$on("toastMessage", this.makeToast);
  },
  beforeDestroy(){
    eventBus.$off("toastMessage",this.makeToast);
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

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
table{
  justify-content: center ;
  margin: 3%;
  padding: 3%;
}
td{
  margin-right: 10%;
}

</style>
