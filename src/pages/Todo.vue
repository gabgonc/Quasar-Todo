<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-prim">
      <q-input
        v-model="newTask"
        @keyup.enter="addtask"
        class="col"
        bg-color="white"
        square
        filled
        placeholder="Add Task"
        dense
      >
        <template v-slot:append>
          <q-btn @click="addtask" dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1 ': task.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTask(index)"
            round
            dense
            color="grey-7"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="150px" color="primary" />
      <div class="text-h5 text-primary text-center">No Tasks</div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  data() {
    return {
      newTask: "",
      tasks: [
        /* {
          title: "get bananas",
          done: false,
        },
        {
          title: "get tomatoes",
          done: false,
        },
        {
          title: "get oranges",
          done: false,
        }, */
      ],
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Would you like to exclude the task?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify("Task Deleted");
        });
    },
    addtask() {
      this.tasks.push({
        title: this.newTask,
        done: false,
      });
      this.newTask = "";
    },
  },
});
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}

.no-tasks {
  opacity: 0.5;
}
</style>
