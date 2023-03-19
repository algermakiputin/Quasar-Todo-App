<template>
  <h6 v-if="!todos.length">No task to display</h6>
  <q-list bordered separator class="todo-container" v-if="todos.length">
    <q-item
      :clickable="!todo.completed"
      :v-ripple="!todo.completed"
      v-for="(todo, index) in todos"
      :key="index"
      @click.stop="
        () => {
          if (!todo.completed) {
            todo.done = !todo.done;
            todo.toggle = !todo.toggle;
          }
        }
      "
    >
      <q-item-section avatar>
        <q-btn
          icon="delete"
          color="primary"
          round
          flat
          dense
          style="font-size: 0.85em; margin-left: 5px"
          clickable
          @click.stop="
            setIndex(index);
            deleteDialog = true;
          "
          v-if="todo.completed"
        />
        <q-checkbox
          v-if="!todo.completed"
          v-model="todo.done"
          :disable="todo.completed"
          class="no-pointer-events"
        ></q-checkbox>
      </q-item-section>
      <q-item-section class="text-left">
        <q-item-label :class="{ done: todo.completed }">{{
          todo.title
        }}</q-item-label>
        <q-item-label caption :class="{ done: todo.completed }">{{
          todo.description
        }}</q-item-label>
      </q-item-section>
      <q-item-section>
        <q-item-label class="text-right" caption>{{ todo.date }}</q-item-label>
      </q-item-section>
    </q-item>
  </q-list>
  <q-dialog v-model="deleteDialog" persistent>
    <q-card>
      <q-card-section class="row items-center">
        <q-avatar icon="question_mark" color="primary" text-color="white" />
        <span class="q-ml-sm">Remove todo(s)?</span>
      </q-card-section>

      <q-card-actions align="right">
        <q-btn
          flat
          label="Delete"
          color="primary"
          @click.stop="
            remove(index);
            deleteDialog = false;
          "
        />
        <q-btn flat label="Cancel" color="default" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>
<script>
import { defineComponent, ref } from "vue";

export default defineComponent({
  name: "TodoList",
  props: ["todos", "remove"],
  data() {
    return {
      index: 0,
    };
  },
  methods: {
    setIndex(index) {
      this.index = index;
    },
  },
  setup() {
    return {
      deleteDialog: ref(false),
    };
  },
});
</script>

<style lang="scss">
.done.q-item__label {
  text-decoration: line-through;
  color: #777;
}
</style>
