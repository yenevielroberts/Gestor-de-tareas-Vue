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

const completar=(id)=>{

  const tareaModificar =tareas.value.find(tarea=> tarea.id===id );

  if(tareaModificar){
    tareaModificar.completado=true;
  }

}

const desmarcar=(id)=>{

  const tareaModificar =tareas.value.find(tarea=> tarea.id===id );

  if(tareaModificar){
    tareaModificar.completado=false;
  }

}

const eliminar=(id)=>{

  const tareaIndex=tareas.value.findIndex( tarea=>tarea.id===id);

  if(tareaIndex != -1){
    tareas.value.splice(tareaIndex,1);
  }
}

</script>

<template>
  <h1>Gestor de tareas</h1>

  <input v-model="nomTarea" type="text" id="nomTarea" name="nomTarea" placeholder="Escribe una nueva tarea"/>
  <button @click="agregarTarea">Agregar</button>
  <br></br>
  <label for="checkBoxPendiente">Mostras  pendientes</label>
  <input v-model="checkbox" type="checkbox" name="checkBoxPendiente" id="checkBoxPendiente"/>

<!--Pendientes-->
  <div v-if="checkbox">
     <div v-for="tarea in tareas" :key="tarea.id">
        <div v-if="!tarea.completado" class="containerTareas">
          <p>{{ tarea.nombre }}</p>
          <!--Solo se mostrará el boton de completar si el valor de completado es false-->
          <button class="btns" v-show="!tarea.completado" @click="completar(tarea.id)">Completar</button>
          <button @click="eliminar(tarea.id)">Eliminar</button>
        </div>
    </div>
  </div>
  <!--Muestra todas las tareas-->
  <div v-else>
      <div class="containerTareas" v-for="tarea in tareas" :key="tarea.id" >
        <!--Se aplicara la clase complatada si el valor de completado es true-->
        <p :class="{completada: tarea.completado}">{{ tarea.nombre }}</p>
        <!--Solo se mostrará el boton de descarmar si el valor de completado es true-->
        <button v-show="tarea.completado" @click="desmarcar(tarea.id)"> Desmarcar</button>
         <!--Solo se mostrará el boton de completar si el valor de completado es false-->
        <button class="btns" v-show="!tarea.completado" @click="completar(tarea.id)">Completar</button>
        <button @click="eliminar(tarea.id)" >Eliminar</button>
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

.completada{
  text-decoration-line: line-through;
}
</style>
