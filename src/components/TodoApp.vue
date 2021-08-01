<template>
  <div class="container">
    <h1 class="text-center mt-5">My first Vue Todo-App</h1>

    <div class="d-flex">
      <input v-model="task" type="text" placeholder="Enter task" class="form-control" />
      <button
        class="btn btn-warning rounded-0"
        @click="submitTask"
      >
        Submit
      </button>
    </div>

    <table class="table table-bordered mt-5">
      <thead>
      <tr>
        <th scope="col">Task</th>
        <th scope="col">Status</th>
        <th scope="col" class="text-center">#</th>
        <th scope="col" class="text-center">#</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(task, index) in tasks" :key="index">
        <td>
          <span :class="{'completed': task.status === 'completed'}">
            {{task.name}}
          </span>
        </td>
        <td style="width: 120px">
          <span
             class="pointer"
             @click="changeStatus(index)"
             :class="{
              'text-danger': task.status === 'to-do',
              'text-warning': task.status === 'in-progress'
             }"
          >
            {{ firstCharUpper(task.status) }}
          </span>
        </td>
        <td id="update">
          <div class="text-center" @click="editTask(index)">
            <span class="fa fa-pen"></span>
          </div>
        </td>
        <td id="delete">
          <div class="text-center" @click="deleteTask(index)">
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
  name: 'TodoApp',
  props: {
    msg: String
  },

  data(){
    return {
      task: '',
      editedTask: null,
      availableStatusList: ['to-do', 'in-progress', 'completed'],
      tasks: [
        {
          name: 'Steal bananas from the store',
          status: 'to-do'
        },
        {
          name: 'Eat 1kg chocolate in 1 hour',
          status: 'in-progress'
        }
      ]
    }
  },

  methods: {
    submitTask() {
      if(this.task.length === 0) return;
      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: 'to-do'
        })
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null
      }


      this.task = ''
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    editTask(index) {
      this.task = this.tasks[index].name

      this.editedTask = index
    },

    changeStatus(index) {
      let newIndex = this.availableStatusList.indexOf(this.tasks[index].status)

      if(++newIndex > 2) newIndex = 0;

      this.tasks[index].status = this.availableStatusList[newIndex]
    },

    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1)
    }
  }
}
</script>

<style scoped>
  .pointer {
    cursor: pointer;
  }
  .completed {
    text-decoration: line-through;
  }
</style>
