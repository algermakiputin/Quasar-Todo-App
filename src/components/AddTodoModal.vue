<template>
  <q-btn label="Add New" color="primary" @click="inception = true" />
  <q-dialog v-model="inception">
    <q-card style="width: 450px; max-width: 80vw">
      <q-card-section>
        <div class="text-h6">Add New ToDo</div>
      </q-card-section>
      <q-card-section class="q-pt-none">
        <q-form class="q-gutter-md">
          <q-input
            filled
            v-model="title"
            label="Title*"
            :rules="[(val) => !!val || 'Field is required']"
          />
          <q-input
            filled
            v-model="description"
            label="Description*"
            :rules="[(val) => !!val || 'Field is required']"
          />
          <q-date v-model="todoDate" />
        </q-form>
      </q-card-section>

      <q-card-actions
        align="left"
        class="text-primary"
        style="margin-left: 8px; margin-bottom: 20px"
      >
        <q-btn color="secondary" label="Save" @click="submit()" />
        <q-btn label="Cancel" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>

  <q-dialog
    v-model="secondDialog"
    persistent
    transition-show="scale"
    transition-hide="scale"
  >
    <q-card style="width: 300px">
      <q-card-section>
        <div class="text-h6 text-center">Task added successfully</div>
      </q-card-section>
      <q-card-actions align="center" class="bg-white text-teal">
        <q-btn flat label="OK" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script>
import { ref } from "vue";
import { date } from "quasar";
export default {
  setup() {
    return {
      inception: ref(false),
      secondDialog: ref(false),
      todoDate: ref(date.formatDate(new Date(), "YYYY/MM/DD")),
      title: ref(""),
      description: ref(),
    };
  },
  inheritAttrs: false,
  props: ["addTodo"],
  methods: {
    submit() {
      if (this.title && this.description && this.todoDate) {
        this.secondDialog = true;
        this.addTodo(this.title, this.description, this.todoDate);
        this.inception = false;
        this.resetFormData();
      } else {
        alert("All required field must be filled out");
      }
    },
    resetFormData() {
      this.title = ref("");
      this.description = ref("");
    },
  },
};
</script>
