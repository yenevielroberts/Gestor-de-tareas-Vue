<script setup>
import {ref, reactive, computed} from 'vue'

const nomTarea=ref('');
const checkbox=ref(false);

//Tareas iniciales
const tareas=ref([
  {id:1, nombre:'Aprender Vue', completado:false},
  {id:2, nombre:'Aprender React', completado:true}
])

//Función para agregar nuevas tareas
const agregarTarea=()=>{

    //Si la variable nomTarea tiene un valor la agrego la tarea
    if(!nomTarea.value) return 

      const nuevaTarea=reactive({
        id:tareas.value.length+1,
        nombre:nomTarea.value,
        completado:false
      })

      tareas.value.push(nuevaTarea)
      nomTarea.value=''//reasigno el valor del nombre
}

//Función para marcar las tareas como completadas
const completar=(id)=>{

  const tareaModificar =tareas.value.find(tarea=> tarea.id===id );

  if(tareaModificar){
    tareaModificar.completado=true;
  }

}

//Función para desmarcar las tareas ya completadas
const desmarcar=(id)=>{

  const tareaModificar =tareas.value.find(tarea=> tarea.id===id );

  if(tareaModificar){
    tareaModificar.completado=false;
  }

}

//Función para eliminar las tareas
const eliminar=(id)=>{

  const tareaIndex=tareas.value.findIndex( tarea=>tarea.id===id);

  if(tareaIndex != -1){
    tareas.value.splice(tareaIndex,1);
  }
}

const totalTareas=computed(()=> {
  const cantidadTareas=tareas.value.length
  return cantidadTareas

})

const totalPendientes=computed(()=>{

  let tareasPendiente=0

  for(let tarea of tareas.value){

    if(!tarea.completado){
      tareasPendiente++
    }
  }

  return tareasPendiente
})

</script>

<template>
<div id="container">
  <h1>Gestor de tareas</h1>

  <section class="sectionAgregarTarea">
    <input v-model="nomTarea" type="text" id="nomTarea" name="nomTarea" placeholder="Escribe una nueva tarea"/>
    <button id="btnAgregar" @click="agregarTarea">Agregar</button>
    <br></br>
    <label for="checkBoxPendiente">Mostras  pendientes</label>
    <input v-model="checkbox" type="checkbox" name="checkBoxPendiente" id="checkBoxPendiente"/>
 </section>
<!--Pendientes-->
  <section class="listaTareas" v-if="checkbox">
     <div v-for="tarea in tareas" :key="tarea.id">
        <div v-if="!tarea.completado" class="containerTareas">
          <p>{{ tarea.nombre }}</p>
          <!--Solo se mostrará el boton de completar si el valor de completado es false-->
          <button class="btnsCompletar" v-show="!tarea.completado" @click="completar(tarea.id)">Completar</button>
          <button class="btnsEliminar" @click="eliminar(tarea.id)">Eliminar</button>
        </div>
    </div>
  </section>
  <!--Muestra todas las tareas-->
  <section class="listaTareas" v-else>
      <div class="containerTareas" v-for="tarea in tareas" :key="tarea.id" >
        <!--Se aplicara la clase complatada si el valor de completado es true-->
        <p :class="{completada: tarea.completado}">{{ tarea.nombre }}</p>
        <!--Solo se mostrará el boton de descarmar si el valor de completado es true-->
        <button v-show="tarea.completado" @click="desmarcar(tarea.id)"> Desmarcar</button>
         <!--Solo se mostrará el boton de completar si el valor de completado es false-->
        <button class="btnsCompletar" v-show="!tarea.completado" @click="completar(tarea.id)">Completar</button>
        <button class="btnsEliminar" @click="eliminar(tarea.id)" >Eliminar</button>
      </div>
  </section>
 <div class="informe">
  <p>Total: {{ totalTareas }} |</p>
  <p>Pendientes {{ totalPendientes }}</p>
 </div>

</div>
</template>

<style scoped>

#container{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

/**Sesion pra agregar tareas */

input[type='text']{

  border-radius: 10px;
  width: 200px;
  height: 20px;
  margin-bottom: 50px;
}

#btnAgregar{
  border-radius: 10px;
  background-color: #228B22;
  padding:5x;
}

#btnAgregar:hover{
  color: white;
}

/**Contenedor individual de las tareas */
.containerTareas{

  display: flex;
  flex-direction: row;
  padding: 5px;
  margin: 10px;
  align-items: center;
  border-radius: 10px;
  background-color: #008B8B;
  color:white;
  font-weight: 600;
}

/**Botones */
button{

  margin:5px;
  padding:5px;
  height: fit-content;
  background-color: transparent;
  border: none;
  font-weight: 700;
}

.btnsEliminar:hover{
  font-weight: 700;
  color:red;
}

.btnsCompletar:hover{

  font-weight: 700;
  color:greenyellow;
}


.completada{
  text-decoration-line: line-through;
}


.informe{
  display: flex;
  flex-direction: row;
}

.informe p{
  margin-right: 5px;
  font-weight: bold;
}
</style>
