<script setup>
const props = defineProps([
     'tarefaDuplicada',
     'tarefas',
     'quantidadeTarefas',
     'fecharAlertaDuplicado',
     'tarefaTemp',
]);
</script>
<template>
     <ul class="list-group mt-4">
          <!-- Alertas segundo -->
          <div
               v-show="props.tarefaDuplicada"
               class="alert alert-danger alert-dismissible fade show"
               role="alert"
          >
               A tarefa <strong>{{ props.tarefaTemp }}</strong> já foi adicionada.
               <button
                    type="button"
                    class="btn-close"
                    aria-label="Close"
                    @click="props.fecharAlertaDuplicado"
               ></button>
          </div>

          <!-- Alertas primeiro -->
          <div v-if="props.quantidadeTarefas === 0" class="alert alert-warning" role="alert">
               Você ainda não possui nenhuma tarefa!
          </div>

          <li v-else class="list-group-item" v-for="tarefa in props.tarefas">
               <input
                    @change="evento => (tarefa.finalizada = evento.target.checked)"
                    :checked="tarefa.finalizada"
                    :id="tarefa.titulo"
                    type="checkbox"
               />
               <label :for="tarefa.titulo" :class="{ done: tarefa.finalizada }" class="ms-3">
                    {{ tarefa.titulo }}
               </label>
          </li>
     </ul>
</template>

<style scoped>
.done {
     text-decoration: line-through;
}
</style>
