<template>
  <q-item
    clickable
    v-ripple
    @click="updateTask({id, updates: {complete: !task.complete}})"
    :class="task.complete ? 'text-grey-5' : ''"
  >
    <q-item-section side top>
      <q-checkbox v-model="task.complete" class="no-pointer-events"/>
    </q-item-section>

    <q-item-section>
      <q-item-label>{{ task.name }}</q-item-label>
    </q-item-section>

    <q-item-section side top v-if="task.date">
      <q-item-label
        caption
        :class="task.complete ? 'text-grey-5' : ''"
      >
        <q-icon name="event"/>
        {{ task.date }}
      </q-item-label>
    </q-item-section>

    <q-item-section side>
      <q-btn flat round dense icon="edit"
             @click.stop="showEditTask = true"/>
    </q-item-section>

    <q-item-section side>
      <q-btn flat round dense icon="delete"
             @click.stop="promptToDelete(id)"/>
    </q-item-section>

    <q-dialog v-model="showEditTask">
      <EditTask :task="task" :id="id" @close="showEditTask = false"></EditTask>
    </q-dialog>

  </q-item>
</template>

<script>
  import {mapMutations} from 'vuex';
  import {deleteTask} from "src/store/module-tasks/mutations";
  import EditTask from "components/EditTask";

  export default {
    name: "Task",
    components: {EditTask},
    props: {
      'task': {
        required: true,
        type: Object,
      },
      'id': {
        required: true,
        type: String,
      }
    },

    data() {
      return {
        showEditTask: false,
      }
    },

    methods: {
      ...mapMutations('tasks', ['updateTask', 'deleteTask']),

      promptToDelete(id) {
          this.$q.dialog({
            dark: true,
            title: 'Confirm',
            message: 'Are you sure you want to delete this task?',
            cancel: true,
            persistent: true
          }).onOk(() => {
            this.deleteTask(id)
          });
      }
    }
  }
</script>

<style scoped>

</style>
