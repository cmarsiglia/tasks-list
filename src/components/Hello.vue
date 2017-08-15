<template lang="pug">

  .task
    .section
      .container
      .columns.is-mobile
        .column.is-half.is-offset-one-quarter
            h4.title.has-text-centered Platzi Tasks
            h4.subtitle NEW TASK
            .column
            .field.has-addons
              p.control
                  input.input(type='text', v-model="newTask.title", placeholder='Add Tarea')
              p.control
                  input.input(type='number', v-model="newTask.time")
              p.control
                  a.button.is-primary(@click="addTask") Add Task

            p.is-clearfix

            h4.title.has-text-centered TASKS LIST | Total hours: {{sumTime}}
            .list-tasks(v-if="tasks.length > 0")
              .notification(v-for="(task, indice) in tasks")
                button.delete(@click="deleteTask(indice)")
                p {{ task.title }}
                strong Hours: {{ task.time }}

            .list-not-tasks(v-else)
              p No hay datos




</template>

<script>
export default {
  name: 'hello',
  data () {
    return {
      name: '',
      tasks: [],
      newTask:
        {
          title: '',
          time: 0
        }

    }
  },
  created () {
    // Obtenemos el array en Formato JSON de localStorage
    this.tasks = JSON.parse(localStorage.getItem('tasks')) || []

  },
  methods:{
    addTask(){
        if (this.newTask.title !='' && this.newTask.time > 0) {

        let row = {
          title: this.newTask.title,
          time: this.newTask.time
        }

        // Llenamos el array tasks[] con la data de title y time.
        this.tasks.push(row)

        // Almacenamos el array en localStorage
        // El método JSON.stringify() convierte un valor dado en javascript a una cadena  JSON
        localStorage.setItem('tasks',JSON.stringify(this.tasks))

        //Limpiamos los input title y time despues de Agregarlos al array tasks[]
        this.newTask.title = ''
        this.newTask.time = 0
      }else{
        alert("Los campos Tarea y Tiempo son requeridos")
      }
   },

   deleteTask(indice){

     console.log(indice)
      this.tasks.splice(indice, 1)
      localStorage.setItem('tasks', JSON.stringify(this.tasks))

   }
  },
  computed:{
    sumTime: function () {
      let time = 0
      this.tasks.forEach(function (task) {
        time += parseInt(task.time)
      })
      return time
    }
 }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
