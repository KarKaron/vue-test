<template>
  <div>
    <h1>List</h1>
    <div class="row valign-wrapper">
      <div class="col s-12 m-6">
        <label>Status Filter</label>
        <select ref="select" v-model="filter">
          <option value="" disabled selected>Choose filter</option>
          <option value="active">Active</option>
          <option value="expired">Expired</option>
          <option value="completed">Completed</option>
        </select>        
      </div>
      <div class="col s-12 m-6">
        <button v-if="filter" class="btn btn-small red" @click="filter = null">Clear filter</button>
      </div>
    </div>
    <hr>
    <table v-if="tasks.length">
      <thead>
        <tr>
          <th>#</th>
          <th>Name</th>
          <th>Date</th>
          <th>Description</th>
          <th>Status</th>
          <th>Open</th>
        </tr>
      </thead>
      <tbody>
        <tr 
          v-for="(task, idx) of displayTasks"
          :key=task.id
        >
          <td>{{idx + 1}}</td>
          <td>{{task.title}}</td>
          <td>{{new Date(task.date).toLocaleDateString()}}</td>
          <td class="tdWidth">
            <div class="text">{{task.description}}</div>
          </td>
          <td><span class="new badge" :data-badge-caption=task.status></span></td>
          <td>
            <router-link tag="button" class="btn btn-small blue darken-3" :to="'/task/'+task.id">
              Open
            </router-link>
          </td>
        </tr>
      </tbody>
    </table>
    <p v-else>No Tasks!</p>
  </div>
</template>

<script>
export default {
  data: () => ({
    filter: null
  }),
  computed: {
    tasks() {
      return this.$store.getters.tasks
    },
    displayTasks() {
      return this.tasks.filter(t => {
        if (!this.filter) {
          return true
        }
        return t.status === this.filter
      })
    }
  },
  mounted() {
    M.FormSelect.init(this.$refs.select)
  }
}
</script>

<style lang="scss" scope>
  .tdWidth {
    max-width: 400px;
  }

  .text {
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
  }
</style>