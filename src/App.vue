<template>
  <div id="panel">
    <div class="header">
      <h2>Lista de tarefas</h2>
      <input type="text" placeholder="Adicionar sua tarefa" v-model="task" @keyup.enter="addTask(task)" maxlength="50">
      <button type="button" @click="addTask(task)">Adicionar</button>
      <p>{{ countTaskCompleted}} / {{ tasks.length }}</p>
    </div>
    <div class="container">
      <div v-if="tasks.length > 0">
        <app-task v-for="task in tasks" :key="task.id" :item="task" @completed="changeStatusTask" @remove="delTask"/>
      </div>
      <p v-else>Sem tarefas pendentes</p>
    </div>
  </div>
</template>

<script>
  import Task from '@/components/Task.vue'
  export default {
    name: 'App',
    components: {
      'app-task': Task
    },
    data(){
      return {
        task: "",
        tasks: []
      }
    },
    computed: {
      countTaskCompleted(){
        return this.tasks.filter((item) => item.isCompleted).length;
      }
    },
    methods: {
      addTask(item){
        if(item.trim().length > 0){
            this.tasks.push({
            id: this.tasks.length,
            task: item,
            isCompleted: false
          });
          this.task = "";
        }
        this.updateLocalStorage();
      },
      changeStatusTask(id){
        this.tasks.map((item) => {
          if(item.id === id){
            item.isCompleted = !item.isCompleted
          }
        });
        this.updateLocalStorage();
      },
      delTask(id){
        this.tasks = this.tasks.filter((item) => item.id !== id);
        this.updateLocalStorage();
      },

      updateLocalStorage(){
        localStorage.setItem('todo-list-vuejs', JSON.stringify(this.tasks));
      }
    },
    created(){
      let data = JSON.parse(localStorage.getItem('todo-list-vuejs'));
      if(Array.isArray(data)){
        this.tasks = data;
      }
    }
  }
</script>

<style scoped>
  #panel {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  #panel > .container {
    width: 500px;
  }

  #panel > .container > p{
    text-align: center;
  }
</style>
