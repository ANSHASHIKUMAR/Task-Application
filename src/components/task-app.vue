<template>
  <div class="container " style="font-family: 'YourFontFamily', sans-serif;">
    <h2 class="text-center mt-5">Task Application</h2>
    <!--Input-->
    <div class="d-flex mt-3">
      <input  v-model="taskTitle" type="text" placeholder="Enter title" class="form-control" @input="hideAlert" />
      <input  v-model="taskDescription" type="text" placeholder="Enter task" class="form-control mx-2" @input="hideAlert"/>
      <button @click="submitTask" class="btn btn-secondary rounded-2 mx-1" :disabled="isInputEmpty ">SUBMIT</button>  
    </div>
    <!-- Validation msg -->
    <div v-if="isInputEmpty" class="alert alert-danger mt-2">{{ alertMessage }}</div>
    <!-- buttons to show the tasks based on the  status  -->
    <div class=" container d-flex mt-4 justify-content-between flex-wrap">
      <div class="col-12 col-md-5">
        <div class="btn-group btn-group-toggle">
        <button @click="setFilter('finished')" class="btn btn-success rounded-2">Completed</button>
        <button @click="setFilter('in-progress')" class="btn btn-warning rounded-2 mx-1">In-completed</button>
        <button @click="setFilter('all')" class="btn btn-info rounded-2 mx-1">All tasks</button>
        </div>
      </div>
      <div class="form-group mt-1 col-12 col-md-2">
        <input type="text" v-model="search" placeholder="Filter tasks" class="form-control rounded-2 ">
      </div>
    </div>
    <!--task table-->
    <table class="table table-bordered mt-3" >
      <thead>
        <tr>
          <th scope="col">Task Title</th>
          <th scope="col">Task</th>
          <th scope="col" class="text-center">Status</th>
          <th scope="col" class="text-center">Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in filteredTasks" :key="index">
          <td :class="{
            'text-warning': task.status === 'in-completed',
            'text-primary': task.status === 'in-progress',
            'text-success': task.status === 'completed',
          }">{{ task.title }}</td>
          <td :class="{
            'text-warning': task.status === 'in-completed',
            'text-primary': task.status === 'in-progress',
            'text-success': task.status === 'completed',
          }">{{ task.description }}</td>
          <td style="width: 150px" class="text-center" :class="{
            'text-warning': task.status === 'in-completed',
            'text-primary': task.status === 'in-progress',
            'text-success': task.status === 'completed',}">
            <span @click="changeStatus(index)" class="pointer border border-none px-2 py-1 rounded">
              {{firstCharUpper(task.status)}}</span></td>
          <td style="width: 150px">
            <div class="text-center"
              @click="deleteTask(index)" :class="{
                'text-warning': task.status === 'in-completed',
                'text-primary': task.status === 'in-progress',
                'text-success': task.status === 'completed',}">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
export default {
  name:"HelloWorld",
  props:{
    msg:String,
  },
  methods: {
    submitTask(){
      if (this.taskTitle.length === 0 || this.taskDescription.length === 0) {
      this.isInputEmpty = true;
      this.alertMessage = 'Please enter a task before submitting.';
      return;
    }
    this.tasks.push({
      title:this.taskTitle,
      description:this.taskDescription,
      status:'in-progress'
      });
      this.task= '';
      this.taskTitle = '';
      this.taskDescription= '';
    },
    hideAlert() {
    this.isInputEmpty = false;
    this.alertMessage = '';
    },
    deleteTask(index){
      this.tasks.splice(index, 1);
    },
    changeStatus(index){
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },
    firstCharUpper(str){
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
    setFilter(status) {
      this.selectedFilter = status;
    }  
  },
  computed: {
    filteredTasks() {
    if (this.selectedFilter === 'all') {
      return this.tasks.filter(task => task.status.includes(this.search));
    } else {
      return this.tasks.filter(task => task.status === this.selectedFilter && task.status.includes(this.search));
    }
  }
  },
  data(){
    return{
      task: '',
      availableStatuses:['in-completed','in-progress','completed'],
      search:'',
      selectedFilter: 'all',
      taskTitle: '',
      taskDescription: '',
      tasks: [
        {
          title:'Grocery Shopping',
          description:'Buy milk, eggs, bread, and vegetables.',
          status:'Completed'
        },
        {
          title:'Study for Exam',
          description:'Review chapters 3 to 5 for the math exam.',
          status:'In-completed'
        }
      ],
      isInputEmpty: false,
      alertMessage:'',
      slideOut: false,
    }
  }
};
</script>
<style scoped>
.pointer{
  cursor:pointer;
}

</style>
