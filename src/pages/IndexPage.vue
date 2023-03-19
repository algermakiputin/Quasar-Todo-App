<template>
  <q-page class="q-pa-md">
    <h1 class="page-header">My Todo List</h1>
    <p>Click the todo list to mark as done or remove from the list</p>
    <TodoList :todos="todos" :remove="removeTask" />
    <div class="q-gutter-sm">
      <q-btn
        label="Complete"
        color="secondary"
        @click.stop="confirm = true"
        v-if="showButtons()"
      />
      <q-btn
        label="Delete"
        color="deep-orange"
        @click.stop="deleteDialog = true"
        v-if="showButtons()"
      />
      <AddTodoModal :addTodo="addTask" />
    </div>
    <q-dialog v-model="confirm" persistent>
      <q-card>
        <q-card-section class="row items-center">
          <q-avatar icon="question_mark" color="primary" text-color="white" />
          <span class="q-ml-sm">Mark todo as completed?</span>
        </q-card-section>
        <q-card-actions align="right">
          <q-btn
            flat
            label="Confirm"
            color="primary"
            @click.stop="completeTasks()"
          />
          <q-btn flat label="Cancel" color="default" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>
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
            @click.stop="deleteTasks()"
          />
          <q-btn flat label="Cancel" color="default" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </q-page>
</template>

<script>
import { defineComponent, ref } from "vue";
import { date } from "quasar";
import TodoList from "src/components/TodoList.vue";
import AddTodoModal from "src/components/AddTodoModal.vue";
export default defineComponent({
  name: "IndexPage",
  components: {
    TodoList,
    AddTodoModal,
  },
  setup() {
    return {
      confirm: ref(false),
      deleteDialog: ref(false),
      date: new Date(),
    };
  },
  data() {
    return {
      todos: [
        {
          title: "Book flight to Iraq",
          description: "broombrm",
          date: "2023-03-25",
          done: true,
          completed: true,
          toggle: false,
        },
        {
          title: "Buy dog foods",
          description: "feed the dog",
          date: "2023-03-20",
          done: true,
          completed: true,
          toggle: false,
        },
        {
          title: "Wash the dishes",
          description: "remove the stains",
          date: "2023-03-18",
          done: true,
          completed: true,
          toggle: false,
        },
        {
          title: "Drink water",
          description: "stay hyrdated all day",
          date: "2023-03-18",
          done: false,
          completed: false,
          toggle: false,
        },
        {
          title: "Play some games",
          description: "puzzle bubble",
          date: "2023-03-18",
          done: false,
          completed: false,
          toggle: false,
        },
      ],
    };
  },
  methods: {
    setOption(option) {
      this.option = option;
    },
    removeTask(index) {
      this.todos.splice(index, 1);
    },
    addTask(title, description, date) {
      this.todos.push({
        title,
        description,
        date,
        done: false,
        completed: false,
        toggle: false,
      });
    },
    completeTasks() {
      this.todos = this.todos.map((todo) => ({
        ...todo,
        done: todo.done,
        completed: todo.done && (todo.toggle || todo.completed),
        toggle: false,
      }));
      this.confirm = false;
    },
    deleteTasks() {
      this.todos = this.todos.filter((todo) => !todo.toggle);
      this.deleteDialog = false;
    },
    showButtons() {
      return this.todos.find((todo) => todo.toggle);
    },
  },
});
</script>

<style lang="scss">
.todo-container {
  margin-bottom: 20px !important;
}
.page-header {
  font-size: 2.2em;
  margin: none;
  padding: 0;
  line-height: 0.85em;
}
</style>
