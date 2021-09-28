<template>
<div class="row">
  <div class="col s6 offset-s3">
    <div v-if="task" >
      <h1>{{task.title}}</h1>
    </div>
    <div v-else>Task Not Found!</div>

    <div>
        <form @submit.prevent="submitHandler">

          <div class="chips" ref="chips"></div>

          <div class="input-field">
            <textarea v-model="description" id="description" class="materialize-textarea"></textarea>
            <label for="description">Textarea</label>
            <span class="character-counter" style="float: right; font-size: 12px;">{{description.length}}/2048</span>
          </div>

          <input type="text" ref="datepicker">
          
          <div v-if="task.status !== 'completed'">
            <button class="btn" type="submit" style="margin-right:20px">Update</button>
            <button class="btn blue" type="button" @click="CompleteTask"> Complete task</button>
          </div>
         
        </form>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name:'tasks',
  computed:{
    task(){
      return this.$store.getters.taskById(+this.$route.params.id)
    }
  },
  data:() => ({
      description:'',
      chips:null,
      date:null
    }),
    methods:{
      submitHandler(){
        this.$store.dispatch('updateTask', {
          id:this.task.id,
          description:this.description,
          date:this.date.date
        })
        this.$router.push('/list')
      },
      CompleteTask(){
        this.$store.dispatch('completeTask', this.task.id)
        this.$router.push('/list')
      }
      
    },
    destroyed(){
      if(this.chips && this.chips.destroyed) {
        this.chips.destroyed()
      }
      
      if(this.date && this.date.destroyed){
        this.date.destroyed()
      }
    },
    mounted(){
      this.description = this.task.description
      this.chips = M.Chips.init(this.$refs.chips, {
        placeholder:'Task tags',
        data:this.task.tags
      })
  
      this.date = M.Datepicker.init(this.$refs.datepicker, {
        format:'dd.mm.yyyy',
        defaultDate:new Date(this.task.date),
        setDefaultDate:true
      })
      setTimeout(() => {
        M.updateTextFields()
      },0)
    }
};
</script>

<style>
    
</style>