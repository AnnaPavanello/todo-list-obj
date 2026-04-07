<script setup>
import { ref } from 'vue'
const tarefas = ref([
  { id: 1, desc: 'Prova Geografia', status: 'pendente' },
  { id: 2, desc: 'Prova História', status: 'concluida' },
  { id: 3, desc: 'Trabalho DevWeb', status: 'pendente' }
]);

const novaTarefa = ref('')
const posicaoAlterar = ref(-1)
function addTarefa() {
  if (posicaoAlterar.value == -1) {
    let maiorID = Math.max(...tarefas.value.map(item => item.id));
    tarefas.value.push({ id: maiorID + 1, desc: novaTarefa.value, status: 'pendente' });
    novaTarefa.value = '';
  }
  else {
    tarefas.value[posicaoAlterar.value].desc = novaTarefa.value
    posicaoAlterar.value = -1
  }
  novaTarefa.value = ''
}
function deletarTarefa(item) {
  const posicao = tarefas.value.findIndex(t => t.id === item.id);
  tarefas.value.splice(posicao, 1);
}
function editarTarefa(item) {
  posicaoAlterar.value = tarefas.value.findIndex(t => t.id === item.id);
  novaTarefa.value = tarefas.value[posicaoAlterar.value].desc
}
function marcarConcluida(id){
  const posicao = tarefas.value.findIndex(t => t.id == id);
  if (tarefas.value[posicao].status === 'concluida'){
    tarefas.value[posicao].status = 'pendente'
  }
  else {
    tarefas.value[posicao].status = 'concluida'
  }
}
</script>

<template>

  <div class="container">
    <input type="text" v-model="novaTarefa">
    <button @click="addTarefa">Adicionar</button>
    <ul>
      <li v-for="item in tarefas" :key="item.id">
        <span @click="marcarConcluida(item.id)" :class="{concluida: item.status == 'concluida'}">
          {{ item.desc }}
        </span>
        
        <span>
          <a href="#" @click.prevent="deletarTarefa(item)">Delete</a>
          <a href="#" @click.prevent="editarTarefa(item)">Edit</a>
        </span>
      </li>
    </ul>
  </div>
</template>

<style scoped>
li{
  cursor: pointer;
}
.concluida{
  text-decoration: line-through;
}
</style>
