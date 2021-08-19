<template>
  <div class="container" style="max-width: 600px">
    <h1 class="text-center at-5">NewYear Resolution</h1>
    <h4 class="text-center at-5">2021</h4>
    <div class="d-flex">
      <input
        v-model="task"
        type="text"
        placeholder="What needs to be done? Plan Now!"
        class="form-control"
      />
      <button @click="addResolution" class="btn btn-outline-danger">ADD</button>
    </div>

    <table class="table" style="margin-top: 30px">
      <thead class="thead-dark bg-warning">
        <tr>
          <th scope="col">Resolution</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">Edit</th>
          <th scope="col" class="text-center">Remove</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasksFiltered" :key="index">
          <td>
            <span :class="{ finished: task.status === 'finished' }">{{
              task.name
            }}</span>
          </td>
          <td style="width: 100px">
            <span
              @click="changeStatus(index)"
              class="pointer"
              :class="{
                'text-danger': task.status === 'to-do',
                'text-warning': task.status === 'in-progress',
                'text-success': task.status === 'finished',
              }"
              >{{ firstCharUpper(task.status) }}</span
            >
          </td>
          <td>
            <div class="text-center" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
      <tfoot>
        <tr>
          <td>
            <span
              :class="{ active: filter === 'showAll' }"
              @click="filter = 'showAll'"
              >All</span
            >
          </td>
          <td>
            <span
              :class="{ active: filter === 'active' }"
              @click="filter = 'active'"
              >Active</span
            >
          </td>
          <td>
            <span
              :class="{ active: filter === 'inProgress' }"
              @click="filter = 'inProgress'"
              >On-Progress</span
            >
          </td>
          <td>
            <span
              :class="{ active: filter === 'finished' }"
              @click="filter = 'finished'"
              >Finished</span
            >
          </td>
        </tr>
      </tfoot>
    </table>
  </div>
</template>

<script>
export default {
  name: "NewYear",
  props: {
    msg: String,
  },
  data() {
    return {
      task: "",
      editedTask: null,
      availableStatuses: ["to-do", "in-progress", "finished"],
      filter: "all",
      tasks: [
        {
          name: "Exercise and Get Fit",
          status: "to-do",
        },
        {
          name: "Get a Job",
          status: "in-progress",
        },
        {
          name: "Save Money",
          status: "in-progress",
        },
        {
          name: "Learn a new Language",
          status: "to-do",
        },
        {
          name: "Stop Over-Sleeping",
          status: "finished",
        },
      ],
    };
  },
  computed: {
    tasksFiltered() {
      if (this.filter === "showAll") {
        return this.tasks;
      } else if (this.filter === "active") {
        return this.tasks.filter((task) => !task.status);
      } else if (this.filter === "inProgress") {
        return this.tasks.filter((task) => task.status);
      } else if (this.filter === "finished") {
        return this.tasks.filter((task) => task.status);
      } else return this.tasks;
    },
  },
  methods: {
    addResolution() {
      if (this.task.trim().length === 0) return;
      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: "to-do",
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }
      this.task = "";
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },
    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pointer {
  cursor: pointer;
}
.pointer:hover {
  color: #109bc9;
}
.finished {
  text-decoration: line-through;
}
</style>
