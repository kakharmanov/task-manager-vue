<template>
  <div>
    <h1>List</h1>
    <div class="row">
      <div class="input-field col s6" >
        <select ref="select" v-model="filter">
          <option value="all" disabled selected>All</option>
          <option value="active">active</option>
          <option value="outdated">outdated</option>
          <option value="completed">completed</option>
        </select>
        <label>Status Field</label>
      </div>
    </div>

    <!-- <hr> -->
    
    <table v-if="tasks.length">
      <thead>
        <tr>
          <th>#</th>
          <th>title</th>
          <th>Date</th>
          <th>Descrription</th>
          <th>Status</th>
          <th>Open</th>
        </tr>
      </thead>
      <tbody>
        <tr 
          v-for="(task, i) of displayTasks"
          :key="task.id"
        
        >
          <td>{{i + 1 }}</td>
          <td>{{task.title}}</td>
          <td>{{new Date(task.date).toLocaleDateString()}}</td>
          <td class="widthOfDescription"><div class="text">{{task.description}}</div></td>
          <td>{{task.status}}</td>
          <td>
            <router-link tag="button" class="btn btn-small" :to="'/task/' + task.id">open</router-link>
          </td>
        </tr>
      </tbody>
    </table>
    <p v-else>No tasks</p>
  </div>
</template>

<script>
export default {
    name:'list',
    data: () => ({
      filter:null
    }),
    computed:{
      tasks(){
        return this.$store.getters.tasks
      },
      displayTasks(){
        return this.tasks.filter(t => {
          if(!this.filter) {
            return true 
          }
          else{
            return t.status === this.filter
          }
        })
        
      }
    },
    mounted(){
      M.FormSelect.init(this.$refs.select)
    }
};
</script>

<style scoped>
    .widthOfDescription{
      max-width: 200px;
    }
    .text{
      white-space: nowrap;
      text-overflow: ellipsis;
      overflow: hidden;
    }
</style>