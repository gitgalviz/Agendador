<template>
  <div class="cont_padre">
    <h1>Agendador</h1>
    <div class="cont_actividad">
      <div class="alerta">
        <h3 :style="alerta === 'Registro exitoso' || 'Por favor llene todos los campos' ? [ 'color:black', 'padding:20px'] : ''">{{ alerta }}</h3>
      </div>
      <input type="text" class="actividad" placeholder="Ingrese su actividad" v-model="actividad">
    </div>
    <div class="cont_fecha">
      <h1>Fecha</h1>
      <input type="date" class="fecha" placeholder="Ingrese la fecha" v-model="fecha">
    </div>
    <div class="inputs">
      <button class="btn" @click="ordenar">Ordenar </button>
      <label class="container">
        <input checked="checked" type="checkbox" v-model="check">

        <div class="checkmark"></div>
      </label>
      <button class="btn" @click="enviar">Enviar</button>
    </div>
    <div class="tablas">
      <table class="tg">
        <colgroup>
          <col style="width: 33.3%">
          <col style="width: 33.3%">
          <col style="width: 33.3%">
        </colgroup>
        <thead>
          <tr>
            <th class="tg-qk6s">Actividad</th>
            <th class="tg-qk6s">Prioridad</th>
            <th class="tg-qk6s">Fecha</th>
            <th class="tg-qk6s">Opciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in actividades" :key="index" :style = "item.prioridad>'alta'? '' : ['color:red', 'font-weight: bold;', 'font-size: 30px', 'text-transform: uppercase' ]">
            <td >{{ item.actividad }}</td>
            <td class="tg-0lax">{{ item.prioridad }}</td>
            <td class="tg-0lax">{{ item.fecha }}</td>
            <td>
            <button @click="eliminar(i)">❌</button>
            <button @click="editar(item, i)">📝</button>
          </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<style scoped>
.cont_padre {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.cont_actividad {
  margin-top: 20px;
}

.actividad {
  width: 300px;
  height: 30px;
}

.cont_fecha {
  margin-top: 20px;
}

.fecha {
  width: 300px;
  height: 30px;
}

.btn {

  padding: 1rem 2.5rem;
  border: none;
  outline: none;
  border-radius: 0.4rem;
  cursor: pointer;
  text-transform: uppercase;
  background-color: rgb(14, 14, 26);
  color: rgb(234, 234, 234);
  font-weight: 700;
  transition: 0.6s;
  box-shadow: 0px 0px 60px #1f4c65;
  -webkit-box-reflect: below 10px linear-gradient(to bottom, rgba(0, 0, 0, 0.0), rgba(0, 0, 0, 0.4));
}

.btn:active {
  scale: 0.92;
}

.btn:hover {
  background: rgb(2, 29, 78);
  background: linear-gradient(270deg, rgba(2, 29, 78, 0.681) 0%, rgba(31, 215, 232, 0.873) 60%);
  color: rgb(4, 4, 38);
}

.container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

.container {
  display: block;
  position: relative;
  cursor: pointer;
  font-size: 30px;
  user-select: none;
}

.checkmark {
  position: relative;
  top: 0;
  left: 0;
  height: 1.6em;
  width: 1.6em;
  border-radius: 50%;
  background: #000000;
  transition: all 0.2s ease;
}

.checkmark {
  opacity: 0.4;
}

.container input:checked~.checkmark {
  background: linear-gradient(144deg, #be63ff, #b9b9b9 50%, #000000);
  opacity: 0.9;
  transition: all 0.2s ease;
}

.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

.container input:checked~.checkmark:after {
  display: block;
}

.container .checkmark:after {
  left: 0.61em;
  top: 0.43em;
  width: 0.25em;
  height: 0.5em;
  border: solid rgb(255, 255, 255);
  border-width: 0 0.15em 0.15em 0;
  transform: rotate(45deg);
}

.tablas {
  width: 100%;
  margin-top: 20px;
  text-align: center;
}

.tg {
  border-collapse: collapse;
  border-spacing: 0;
  width: 100%;

}

.tg td {
  text-align: center;
  border-color: black;
  border-style: solid;
  border-width: 1px;
  font-family: Arial, sans-serif;
  font-size: 14px;
  overflow: hidden;
  padding: 10px 5px;
  word-break: normal;
}

.tg th {
  border-color: black;
  border-style: solid;
  border-width: 1px;
  font-family: Arial, sans-serif;
  font-size: 14px;
  font-weight: normal;
  overflow: hidden;
  padding: 10px 5px;
  word-break: normal;


}

.tg .tg-qk6s {
  background: linear-gradient(270deg, rgba(2, 29, 78, 0.681) 0%, rgba(31, 215, 232, 0.873) 60%);
  color: #000000;
  font-weight: bold;
  text-align: center;
  vertical-align: top
}

.tg .tg-0lax {
  text-align: left;
  vertical-align: top
}

.inputs {
  display: flex;
  justify-content: space-between;
  width: 300px;
  margin-top: 20px;
  margin-bottom: 120px;
  margin-right: 170px;
  gap: 70px;
  

}

</style>



<script setup>
import { ref } from 'vue';

let actividades = ref([]);

const actividad = ref('');
const fecha = ref('');
const check = ref(false);
let alerta = ref('');

function ocultarAlerta(){
    setTimeout (()=> {
      alerta.value = '';
    }, 3000)
}

function eliminar(i){
  actividades.value.splice(i,1)
}

const enviar = () => {
  if (actividad.value === '' || fecha.value === '') {
    alerta.value = 'Por favor llene todos los campos'
    ocultarAlerta()
    return;
  } else {
    let prioridad = check.value ? 'alta' : 'baja';
    actividades.value.push({
      actividad: actividad.value,
      prioridad: prioridad,
      fecha: fecha.value
    });
    actividad.value = '';
    fecha.value = '';
    check.value = false;
    alerta.value ='Registro exitoso';
    ocultarAlerta() 
    return;
  }
}


const ordenar = () => {

  actividades.value.sort((a, b) => {
    if (a.prioridad === 'alta' && b.prioridad === 'baja') {
      return -1;
    } else if (a.prioridad === 'baja' && b.prioridad === 'alta') {
      return 1;
    } else {

      return 0;
    }
  });
}
</script>
