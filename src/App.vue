<script setup>
import { reactive } from 'vue';
import Cabecalho from './components/Cabecalho.vue';
import Formulario from './components/Formulario.vue';
import ListaDeTarefas from './components/ListaDeTarefas.vue';

const estado = reactive({
     tarefaTemp: '',
     filtro: 'todas',
     tarefas: [],
     tarefaDuplicada: false,
});

const getTarefasPendentes = () => {
     return estado.tarefas.filter(tarefa => !tarefa.finalizada);
};

const getTarefasFinalizadas = () => {
     return estado.tarefas.filter(tarefa => tarefa.finalizada);
};

const getTarefasFiltradas = () => {
     const { filtro } = estado;

     switch (filtro) {
          case 'pendentes':
               return getTarefasPendentes();
          case 'finalizadas':
               return getTarefasFinalizadas();
          default:
               return estado.tarefas;
     }
};

const cadastraTarefa = () => {
     const { tarefas, tarefaTemp } = estado;

     const tarefaNova = {
          titulo: tarefaTemp,
          finalizada: false,
     };

     if (tarefas.some(tarefa => tarefa.titulo === tarefaTemp)) {
          estado.tarefaDuplicada = true;
     } else {
          estado.tarefas.push(tarefaNova);
          estado.tarefaTemp = '';
          estado.tarefaDuplicada = false;
     }
};
</script>

<template>
     <div class="container">
          <Cabecalho :tarefas-pendentes="getTarefasPendentes().length" />
          <Formulario
               :trocar-filtro="evento => (estado.filtro = evento.target.value)"
               :tarefa-temp="estado.tarefaTemp"
               :edita-tarefa-temp="evento => (estado.tarefaTemp = evento.target.value)"
               :cadastra-tarefa="cadastraTarefa"
          />
          <ListaDeTarefas
               :tarefas="getTarefasFiltradas()"
               :quantidade-tarefas="estado.tarefas.length"
               :tarefa-duplicada="estado.tarefaDuplicada"
               :fechar-alerta-duplicado="() => (estado.tarefaDuplicada = false)"
               :tarefa-temp="estado.tarefaTemp"
          />
     </div>
</template>
