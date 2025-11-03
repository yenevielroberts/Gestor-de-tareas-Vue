<script setup>
import {ref, reactive} from 'vue'

const nomTarea=ref('');
const checkbox=ref(false);

//Tareas iniciales
const tareas=ref([
  {id:1, nombre:'Aprender Vue', completado:false},
  {id:2, nombre:'Aprender React', completado:true}
])

const agregarTarea=()=>{

    //Si la variable nomTarea tiene un valor la agrego la tarea
    if(!nomTarea.value) return 

      const nuevaTarea=reactive({
        id:tareas.length+1,
        nombre:nomTarea.value
      })

      tareas.value.push(nuevaTarea)
      nomTarea.value=''//reasigno el valor del nombre
}

</script>

<template>
  <h1>Gestor de tareas</h1>

  <input v-model="nomTarea" type="text" placeholder="Escribe una nueva tarea"/>
  <button @click="agregarTarea">Agregar</button>
  <br></br>
  <label for="checkBoxPendiente">Mostras  pendientes</label>
  <input v-model="checkbox" type="checkbox" name="checkBoxPendiente" id="checkBoxPendiente"/>

<!--Pendientes-->
  <div v-if="checkbox">
     <div v-for="tarea in tareas" :key="tarea.id">
        <div v-if="!tarea.completado" class="containerTareas">
          <p>{{ tarea.nombre }}</p>
          <button>Completar</button>
          <button>Eliminar</button>
        </div>
    </div>
  </div>
  <!--Muestra todas-->
  <div v-else>
      <div class="containerTareas" v-for="tarea in tareas" :key="tarea.id" >
        <p class="nombreTarea">{{ tarea.nombre }}</p>
        <button class="btns">Completar</button>
        <button>Eliminar</button>
      </div>
  </div>
 
</template>

<style scoped>

.containerTareas{

  display: flex;
  flex-direction: row;
  margin-left: 10px;
  align-items: center;
}

button{

  margin:5px;
  padding:5px;
  height: fit-content;
  border-radius: 5px;
  border: 0px;
}
</style>
