<template>
  <AppDrop @drop="moveTaskOrColumn">
    <AppDrag
      class="column"
      :transferData="{
        type: 'column',
        fromColumnIndex: columnIndex,
      }"
    >
      <div class="flex items-center mb-2 font-bold">
        {{ column.name }}
      </div>
      <div class="list-reset">
        <ColumnTask
          v-for="(task, $taskIndex) in column.tasks"
          :key="$taskIndex"
          :board="board"
          :column="column"
          :task="task"
          :columnIndex="columnIndex"
          :taskIndex="$taskIndex"
        />
        <input
          type="text"
          class="block p-2 w-full bg-transparent"
          placeholder="+ Enter new task"
          @keyup.enter="createTask($event, column.tasks)"
        />
      </div>
    </AppDrag>
  </AppDrop>
</template>

<script>
import ColumnTask from './ColumnTask'
import AppDrag from './AppDrag'
import AppDrop from './AppDrop'
import movingTasksAndColumnsMixin from '../mixins/movingTasksAndColumnsMixin'

export default {
  components: {
    ColumnTask,
    AppDrag,
    AppDrop,
  },
  mixins: [movingTasksAndColumnsMixin],
  methods: {
    createTask(event, tasks) {
      this.$store.commit('CREATE_TASK', {
        tasks,
        name: event.target.value,
      })
      event.target.value = ''
    },
  },
}
</script>

<style lang="css">
.column {
  @apply bg-grey-light p-2 mr-4 text-left shadow rounded;
  min-width: 350px;
}
</style>
